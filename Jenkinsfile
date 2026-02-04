pipeline {
    agent any

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
