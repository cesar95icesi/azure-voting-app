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
        // grype es una herramienta de escaneo de vulnerabilidades 
        // diseñada para encontrar vulnerabilidades en contenedores y 
        // aplicaciones empaquetadas. Al integrar grype en un Jenkinsfile, 
        // se busca automatizar el proceso de identificación de vulnerabilidades 
        // conocidas en las dependencias y paquetes de software utilizados, 
        // mejorando así la seguridad del software desarrollado.
        stage('Run Grype') {
            agent {label 'principal'}
            steps {
                // Run Grype scan on the Docker images
                grypeScan autoInstall: false, repName: 'grypeReport_${JOB_NAME}_${BUILD_NUMBER}.txt', scanDest: 'registry:cesarc95/jenkins-masterclass:20240624-222837'
            }
            post{
                always {
                    recordIssues(
                        tools: [grype()], 
                        aggregatingResults: true,
                    )
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