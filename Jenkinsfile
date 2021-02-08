#!/usr/bin/env groovy

pipeline {
    agent any
  
    stages {
        
        stage('Build') {
            steps {
                withGradle {
                    sh './gradlew assemble'
                }
            }
            post {
                success {
                    archiveArtifacts artifacts: 'build/libs/*.jar'
                }
            }
        }
        stage('Test') {
            steps {
                withGradle {
                    sh './gradlew clean test'
                }
            }
            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                }
            }
        }
         stage('PMD') {
            steps {
                withGradle {
                    sh './gradlew check'
                }
            }
            post {
                always {
                    recordIssues(tools: [pmdParser(pattern: 'build/reports/pmd/*.xml')])
                }
            }
        }

    }
}
