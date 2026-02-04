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
                echo "Current working dir: ${PWD}"                               
                chmod +x mvnw                                
                ./mvnw clean package
                '''
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
