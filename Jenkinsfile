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
                sh 'mvn clean package'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                script {
                    try {
                        echo 'Running Unit Tests and Integration Tests using Maven...'
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
                        body: "The Unit and Integration Tests stage succeeded. Please check Jenkins for details."
                }
                failure {
                    mail to: 'gab.dev.student11@gmail.com',
                        subject: "Unit and Integration Tests Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Unit and Integration Tests stage failed. Please check Jenkins for details."
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Performing Code Analysis using SonarQube...'
                sh 'mvn sonar:sonar'
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    try {
                        echo 'Performing Security Scan using OWASP Dependency-Check...'
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
                        body: "The Security Scan stage succeeded. Please check Jenkins for details."
                }
                failure {
                    mail to: 'gab.dev.student11@gmail.com',
                        subject: "Security Scan Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Security Scan stage failed. Please check Jenkins for details."
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to Staging Server...'
                sh "ssh user@${STAGING_SERVER} 'cd /path/to/app && git pull && ./deploy.sh'"
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Running Integration Tests on Staging Environment using Maven...'
                sh 'mvn verify -Pstaging'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to Production Server...'
                sh "ssh user@${PRODUCTION_SERVER} 'cd /path/to/app && git pull && ./deploy.sh'"
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
    }
}
