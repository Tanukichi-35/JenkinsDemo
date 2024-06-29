pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World from github push-2'
            }
        }
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deplying'
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'MyAWS',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]){
                        sh(script: 'aws s3 cp /var/lib/jenkins/workspace/JenkinsPipeline/index.html s3://test-env-jenkins-123/')
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                script {
                    def url = 'https://test-env-jenkins-123.s3.ap-northeast-3.amazonaws.com/index.html'
                    def response = sh(script: "curl -s -o /dev/null -w '%{http_code}' '$url'", returnStdout: true)

                    if (response == '200') {
                        echo 'Test OK'
                    } else {
                        echo response
                        error 'Test NG'
                    }
                }
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing'
            }
        }
    }
}
