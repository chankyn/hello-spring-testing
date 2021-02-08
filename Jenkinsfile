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
                    jacoco(execPattern: 'build/jacoco/*.exec')
                }
            }
        }
        stage('QA') {
            steps {
                withGradle {
                    sh './gradlew check'
                }
                withSonarQubeEnv(credentialsId: '36e0e707-fb68-46ce-a4c2-45ac89328771',installationName: 'local') {
                    withGradle {
                        sh './gradlew sonarqube'
                    }
                }
            }
            post {
                always {
                    recordIssues(
                        tools: [
                            pmdParser(pattern: 'build/reports/pmd/*.xml'),
                            spotBugs(pattern: 'build/reports/spotbugs/*.xml', useRankAsPriority: true)
                        ]
                    )
                }
            }
        }
    }
}
