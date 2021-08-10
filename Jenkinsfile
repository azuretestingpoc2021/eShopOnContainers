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
  }
  
  
  
  
  
  
  
  
  
  
  
// ================================================================================================
// Initialization steps
// ================================================================================================

def initialize() {
   // env.SYSTEM_NAME = "DSO"
    env.AWS_REGION = "us-east-2"
    env.REGISTRY_URL = "785131266845.dkr.ecr.us-east-2.amazonaws.com"
    env.MAX_ENVIRONMENTNAME_LENGTH = 32

    
}

