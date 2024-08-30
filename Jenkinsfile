pipeline {
    agent any

    environment {
        STAGING_SERVER = 'staging.thistask.com'
        PRODUCTION_SERVER = 'production.thistask.com'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the application using Maven...'
                echo 'Tool: Maven'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                script {
                    try {
                        echo 'Running Unit Tests and Integration Tests using Maven...'
                        echo 'Tool: Maven'
                        // Simulate test execution
                        sh 'mvn test'
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
            post {
                success {
                    mail to: 'gab.dev.student11@gmail.com',
                        subject: "Unit and Integration Tests Succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Unit and Integration Tests stage succeeded. Please check Jenkins for details.\n\nLogs:\n${currentBuild.rawBuild.getLog(50).join('\n')}"
                }
                failure {
                    mail to: 'gab.dev.student11@gmail.com',
                        subject: "Unit and Integration Tests Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Unit and Integration Tests stage failed. Please check Jenkins for details.\n\nLogs:\n${currentBuild.rawBuild.getLog(50).join('\n')}"
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Performing Code Analysis using SonarQube...'
                echo 'Tool: SonarQube'
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    try {
                        echo 'Performing Security Scan using OWASP Dependency-Check...'
                        echo 'Tool: OWASP Dependency-Check'
                        // Simulate security scan
                        sh 'mvn dependency-check:check'
                        currentBuild.result = 'SUCCESS'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        throw e
                    }
                }
            }
            post {
                success {
                    mail to: 'gab.dev.student11@gmail.com',
                        subject: "Security Scan Succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Security Scan stage succeeded. Please check Jenkins for details.\n\nLogs:\n${currentBuild.rawBuild.getLog(50).join('\n')}"
                }
                failure {
                    mail to: 'gab.dev.student11@gmail.com',
                        subject: "Security Scan Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Security Scan stage failed. Please check Jenkins for details.\n\nLogs:\n${currentBuild.rawBuild.getLog(50).join('\n')}"
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging Server...'
                echo 'Server: ${STAGING_SERVER}'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running Integration Tests on Staging Environment using Maven...'
                echo 'Tool: Maven'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production Server...'
                echo 'Server: ${PRODUCTION_SERVER}'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
    }
}
