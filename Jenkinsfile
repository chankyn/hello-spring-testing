#!/usr/bin/env groovy
pipeline {
    agent any
  
    tools { 
        jdk 'OpenJDK-15.0.2' 
    }
    stages {
        
        stage('Build') {
            steps {
                withGradle {
                    sh './gradlew assemble'
                }
            }
        }
        stage('Test') {
            steps {
                withGradle {
                    sh './gradlew test'
                }
            }
        }
    }
}
