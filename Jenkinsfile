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
            stages {
        stage('Test') {
            steps {
                sh '''
                    export PATH="/Library/Frameworks/Python.framework/Versions/3.10/bin:$PATH"
                    pytest ./tests/test_sample.py
                '''
            }
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