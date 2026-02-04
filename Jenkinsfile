pipeline {
    agent any

    environment {
        ECR_REPO_NAME = 'ecom/ecom-config-server'
        APP_VERSION = '1.2.0'
        ARTIFACT_VERSION = "${APP_VERSION}:${BRANCH_NAME}-build-${BUILD_NUMBER}"
    }

    stages {
        stage("Build") {
            agent {
                docker {
                    image 'maven:3.9.6-eclipse-temurin-21'
                    args '-e HOME=.'
                }
            }

            steps {
                sh '''
                echo ${ARTIFACT_NAME}               
                echo "Maven build ..."
                echo ${ECR_REPO_URL}
                echo "Current working dir: ${PWD}"                               
                chmod +x mvnw                                
                ./mvnw versions:set -DnewVersion=${ARTIFACT_VERSION}
                ./mvnw clean package -DskipTests
                '''
            }

            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }

        stage('Test') {
            steps {
                sh '''
                ./mvnw test
                '''
            }
        }

        stage("Push to ECR"){
            steps {
                script {
                    docker.withRegistry("https://${ECR_REPO_URL}", "ecr:us-east-1:ecr-credentials") {

                        // 1. Build the Docker image
                        // Note: You need the 'docker' CLI installed in your agent for this to work.
                        // If the maven image doesn't have 'docker', this part will fail next.
                        def customImage = docker.build("${ECR_REPO_URL}/${ECR_REPO_NAME}:${ARTIFACT_VERSION}")

                        // 2. Push the image
                        customImage.push()
                    }
                }
            }
        }
    }
}
