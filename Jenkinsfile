#!groovy

import groovy.json.JsonSlurper
import java.net.URL

pipeline {
    agent any
    options {
            timeout(time: 1, unit: 'DAYS')
            disableConcurrentBuilds()
    }
  stages {
    stage (init) {
      steps { initialize() }
    }
  
    stage("Build and Register Image") {
       agent { label 'ubuntu_micro'}
       steps { script { buildAndRegisterDockerImage() } }
    }  
}
}
  
  
  
  
  
  
  
  
  
  
  
// ================================================================================================
// Initialization steps
// ================================================================================================

def initialize() {
   // env.SYSTEM_NAME = "DSO"
    env.AWS_REGION = "us-east-2"
    env.REGISTRY_URL = "785131266845.dkr.ecr.us-east-2.amazonaws.com"
    env.REPO_NAME = "eshop"
    env.MAX_ENVIRONMENTNAME_LENGTH = 32

    
}

//===========================================================================================================
// Build and Intialization 
//========================================================================================================
def buildAndRegisterDockerImage() {
    def buildResult
    sh "sudo chmod 777 /var/run/docker.sock"
    echo "Connect to registry at ${env.REGISTRY_URL}" 
    sh "aws ecr get-login-password --region ${env.AWS_REGION} | docker login --username AWS --password-stdin ${env.REGISTRY_URL}"
    sh "docker image prune -a"
    //echo "Build ${env.REGISTRY_URL}/${env.IMAGE_NAME}"
    //sh "docker build -t ${env.REGISTRY_URL}/${env.IMAGE_NAME} ."
    sh "docker-compose -f src/docker-compose.yml build "
    sh """echo "REGISTRY=${env.REGISTRY_URL}/${env.REPO_NAME}"> .env """
    //echo "Register ${env.IMAGE_NAME} at ${env.REGISTRY_URL}"
    sh "docker image ls --format '{{.Repository}}' | grep ${env.REPO_NAME} > repolist"
    sh "cat repolist"
    sh ''' for repo in "$(cat repolist); do 
        reponame=$(echo $repo | awk -F/ '{print $1}')
        aws ecr describe-repositories --repository-names ${reponame} || aws ecr create-repository --repository-name ${reponame}
        done
    '''
    //sh "aws ecr describe-repositories --repository-names ${REPO_NAME} || aws ecr create-repository --repository-name ${REPO_NAME}"
    //sh "docker-compose -f src/docker-compose.yml push"
    

    
    sh "docker image prune -a"
    echo "Disconnect from registry at ${env.REGISTRY_URL}"
    sh "docker logout ${env.REGISTRY_URL}"
}

