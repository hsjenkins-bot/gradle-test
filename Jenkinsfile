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
        stage('Upload JAR to S3') {
            steps {
                dir("${env.WORKSPACE}") {
                    pwd();

                    withAWS(region: 'us-east-1', credentials: 'hs-aws') {
                        s3Upload(bucket: "homespotter-listing-images-dev", path: "test/${env.BUILD_NUMBER}/", file: "${env.WORKSPACE}/build/libs/jenkinsfile-test.jar");
                    }
                }
            }
        }
    }
}
