pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                sh "${env.WORKSPACE}/gradlew build"
            }
        }
        stage('Test') {
            steps {
                sh "${env.WORKSPACE}/gradlew test"
            }
        }
    }
}
