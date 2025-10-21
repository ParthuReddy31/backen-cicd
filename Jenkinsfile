pipeline {
    agent { label 'Agent-1' }

    environment {
        PROJECT = 'Expense'
        COMPONENT = 'Backend'
        ENV = 'Prod'
        appVersion = ''
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
            echo 'I will say Failure-Hello on Failure'
        }
        
        // cleanup {
        //     cleanWs() // Deletes the workspace
        // }
    }
}
