pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello form Githun hook trigger'
            }
        }
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'MyAWS',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]){
                        sh(script: 'aws s3 cp /var/lib/jenkins/workspace/JenkinsPipeline/index.html s3://test-env-jenkins-aki/')
                    }
                }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                script {
                    def url = 'https://test-env-jenkins-aki.s3.ap-northeast-1.amazonaws.com/index.html'
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
        stage('Relese') {
            steps {
                echo 'Relesing'
            }
        }
    }
}
