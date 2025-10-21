pipeline {
    agent { label 'Agent-1' }

    environment {
        PROJECT = 'expense'
        COMPONENT = 'backend'
        ENV = 'prod'
        appVersion = ''
        accountId = '463470981697'
    }
    
    options {
        disableConcurrentBuilds()
        timeout(time: 25, unit: 'MINUTES')
    }
    
    stages {
        stage('Read-Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "App Version is ${appVersion}"
                }
            }
        }
        
        stage('Install-dependencies') {
            steps {
                script {
                    sh '''
                        echo "Installing Dependencies"
                        npm install
                    '''
                }
            }
        }
        stage('Building-Docker-Image') {
            steps {
                script {
                    sh '''
                        echo "Building Docker Image"
                        docker build -t parthureddy3/backend:1.0 .
                    '''
                }
            }
        }
        
        stage('Pushing-Docker-Image-to-AWS-ECR') {
            steps {
                script {
                    withAWS(region: 'us-east-1', credentials: 'AWS-Creds') {
                        sh """
                            aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${accountId}.dkr.ecr.us-east-1.amazonaws.com

                            docker build -t ${accountId}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .

                            docker push ${accountId}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                        """
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo 'I will always say There we go.. Hello again!'
        }
        
        success {
            echo 'I will say Build-Hello on Success'
        }
        
        failure {
            echo 'I will Greet you Failure-Hello on Failure'
        }
        
        // cleanup {
        //     cleanWs() // Deletes the workspace
        // }
    }
}
