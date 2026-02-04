pipeline {
    agent {
        docker {
            image 'maven:3.9.6-eclipse-temurin-21'
        }
    }

    stages {
        stage("Build"){
            steps {
                sh '''
                echo $PWD
                ls
                ./mvnw clean package
                '''
            }
        }

        stage('Hello') {
            steps {
                echo 'Hello World US-0006'
            }
        }
    }
}
