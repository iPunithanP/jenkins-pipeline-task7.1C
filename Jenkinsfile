pipeline {
    agent any

    environment {
        DIRECTORY_PATH = '/var/jenkins_home/workspace/project'
        TESTING_ENVIRONMENT = 'staging-env'
        PRODUCTION_ENVIRONMENT = 'Punithan-production'
        RECIPIENT = 'punithanp.dev@gmail.com'
    }

    stages {
        stage('Build') {
            steps {
                echo "Fetching the source code from ${env.DIRECTORY_PATH}"
                echo 'Task: Compile and package the code'
                echo 'Tool: Maven (mvn clean package)'
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                echo 'Task: Run unit tests to ensure the code functions as expected'
                echo 'Task: Run integration tests to ensure components work together'
                echo 'Tools: JUnit for unit tests, Selenium for integration tests'
            }
            post {
                success {
                    emailext(
                        to: "${env.RECIPIENT}",
                        subject: "Unit and Integration Tests: SUCCESS - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Unit and Integration Tests stage completed successfully.\nBuild URL: ${env.BUILD_URL}\nThe console log is attached.",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${env.RECIPIENT}",
                        subject: "Unit and Integration Tests: FAILURE - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Unit and Integration Tests stage FAILED.\nBuild URL: ${env.BUILD_URL}\nThe console log is attached.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Code Analysis') {
            steps {
                echo 'Task: Analyse the code to ensure it meets industry standards'
                echo 'Tool: SonarQube (SonarQube Scanner plugin for Jenkins)'
            }
        }

        stage('Security Scan') {
            steps {
                echo 'Task: Scan the code and dependencies for known vulnerabilities'
                echo 'Tool: OWASP Dependency-Check'
            }
            post {
                success {
                    emailext(
                        to: "${env.RECIPIENT}",
                        subject: "Security Scan: SUCCESS - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Security Scan stage completed successfully.\nBuild URL: ${env.BUILD_URL}\nThe console log is attached.",
                        attachLog: true
                    )
                }
                failure {
                    emailext(
                        to: "${env.RECIPIENT}",
                        subject: "Security Scan: FAILURE - ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: "The Security Scan stage FAILED.\nBuild URL: ${env.BUILD_URL}\nThe console log is attached.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                echo "Task: Deploy the application to the staging server: ${env.TESTING_ENVIRONMENT}"
                echo 'Tool: AWS CLI / AWS CodeDeploy to an AWS EC2 instance'
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                echo 'Task: Run integration tests on staging in a production-like environment'
                echo 'Tool: Selenium'
            }
        }

        stage('Deploy to Production') {
            steps {
                echo "Task: Deploy the application to the production server: ${env.PRODUCTION_ENVIRONMENT}"
                echo 'Tool: AWS CLI / AWS CodeDeploy to an AWS EC2 instance'
            }
        }
    }
}
