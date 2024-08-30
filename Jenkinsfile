pipeline {
    agent any

    environment {
        STAGING_SERVER = 'staging.thistask.com'
        PRODUCTION_SERVER = 'production.thistask.com'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Stage 1: Build - This stage involves building the code using a build automation tool such as Maven to compile and package the application.'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Stage 2: Unit and Integration Tests - This stage runs unit tests to ensure the code functions as expected and integration tests to ensure different components of the application work together. A test automation tool like JUnit can be used.'
            }
            post {
                success {
                    script {
                        def log = currentBuild.rawBuild.getLog(50).join('\n')
                        mail to: 'gab.dev.student11@gmail.com',
                            subject: "Unit and Integration Tests Succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                            body: "The Unit and Integration Tests stage succeeded. Please check Jenkins for details.\n\nLogs:\n${log}"
                    }
                }
                failure {
                    script {
                        def log = currentBuild.rawBuild.getLog(50).join('\n')
                        mail to: 'gab.dev.student11@gmail.com',
                            subject: "Unit and Integration Tests Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                            body: "The Unit and Integration Tests stage failed. Please check Jenkins for details.\n\nLogs:\n${log}"
                    }
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Stage 3: Code Analysis - This stage integrates a code analysis tool like SonarQube to analyze the code and ensure it meets industry standards.'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Stage 4: Security Scan - This stage performs a security scan on the code using a tool like OWASP Dependency-Check to identify any vulnerabilities.'
            }
            post {
                success {
                    script {
                        def log = currentBuild.rawBuild.getLog(50).join('\n')
                        mail to: 'gab.dev.student11@gmail.com',
                            subject: "Security Scan Succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                            body: "The Security Scan stage succeeded. Please check Jenkins for details.\n\nLogs:\n${log}"
                    }
                }
                failure {
                    script {
                        def log = currentBuild.rawBuild.getLog(50).join('\n')
                        mail to: 'gab.dev.student11@gmail.com',
                            subject: "Security Scan Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                            body: "The Security Scan stage failed. Please check Jenkins for details.\n\nLogs:\n${log}"
                    }
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Stage 5: Deploy to Staging - This stage deploys the application to a staging server, such as an AWS EC2 instance, to simulate a production environment.'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Stage 6: Integration Tests on Staging - This stage runs integration tests on the staging environment to ensure the application functions as expected in a production-like environment.'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Stage 7: Deploy to Production - This stage deploys the application to a production server, such as an AWS EC2 instance, to make it available to end-users.'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
    }
}
