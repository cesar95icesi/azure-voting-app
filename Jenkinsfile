/**
 * Jenkins pipeline script for building and testing a Dockerized application.
 */
pipeline {
    agent any
    stages {
        stage('Verify Branch') {
            steps {
                // Print the current Git branch
                echo "$GIT_BRANCH"
            }
        }
        stage('Docker Build') {
            steps {
                // Build Docker images using Docker Compose
                sh(script: 'docker compose build')
            }
        }
        stage('Start App') {
            steps {
                // Start the application containers using Docker Compose
                sh(script: 'docker compose up -d')
            }
        }
        
        stage('Run Tests') {
            steps {
                // Run tests using pytest
                sh(script: 'pytest ./tests/test_sample.py')
            }
            post {
                success {
                    // Print a success message if tests pass
                    echo 'Tests Passed! :)'
                }
                failure {
                    // Print a failure message if tests fail
                    echo 'Tests Failed! :('
                }
            }
        }

        stage('Run Clair') {
            agent {label 'principal'}
            steps {
                // Run Clair scan on the Docker images
                //sh(script: 'docker scan cesarc95/jenkins-masterclass:latest')
                sh(script: 'docker run -p 5432:5432 -d --name db --platform linux/amd64 arminc/clair-db:latest')
                sh(script: 'docker run -p 6060:6060 --link db:postgres -d --name clair --platform linux/amd64 arminc/clair-local-scan:latest')

            }
        }   

        stage('Run Clair Scan'){
            agent {label 'principal'}
            steps {
                sh(script: '/Users/cesar/clair-scanner/clair-scanner --ip=192.168.68.107 cesarc95/jenkins-masterclass:20240624-222837')
            }
        }

    }
    /**
     * This Jenkinsfile defines the pipeline for the Azure Voting App with Redis.
     * It includes a post-build step that stops and removes the application containers using Docker Compose.
     */

    post {
        always {
            // Stop and remove the application containers using Docker Compose
            sh(script: 'docker compose down')
        }
    }
}