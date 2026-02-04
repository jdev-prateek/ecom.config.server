pipeline {
    agent {
        docker {
            image 'maven:3.9.6-eclipse-temurin-21'
            args '-e HOME=.'
        }
    }

    stages {
        stage("Build") {
            steps {
                sh '''                
                echo "Maven build ..."
                echo ${env.ECR_REPO_URL}
                echo "Current working dir: ${PWD}"                               
                chmod +x mvnw                                
                ./mvnw versions:set -DnewVersion=1.0.0-${BRANCH_NAME}-${BUILD_NUMBER}
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
    }
}
