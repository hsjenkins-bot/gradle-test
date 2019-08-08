pipeline {
    agent any
    environment {
    BUILD_NUMBER_NEW = """${sh(
        returnStdout: true,
        script: 'echo $((BUILD_NUMBER+285))'
        )}"""
    }
    stages {
        stage('Build') { 
            steps {
                sh "printenv"
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
                        s3Upload(bucket: "homespotter-releases", path: "test/${env.BUILD_NUMBER_NEW}/", file: "${env.WORKSPACE}/build/libs/jenkinsfile-test.jar");
                    }
                }
            }
        }
        stage('Push changes to jobs server') {
            steps {
                script {
                    step([
                        $class: "RundeckNotifier",
                        includeRundeckLogs: true,
                        jobId: "501a22ee-9d82-47a2-88e2-ec45f63474d0",
                        nodeFilters: "",
                        options: """
                                build_number=285
                                """,
                        rundeckInstance: "HS Rundeck",
                        shouldFailTheBuild: true,
                        shouldWaitForRundeckJob: true,
                        tags: "",
                        tailLog: true
                    ])
                }
            }
        }
    }
}
