pipeline {
    agent any
    environment {
        // These reference Jenkins credentials (of type 'Secret text')
        AWS_ACCESS_KEY_ID     = credentials('aws-acc-key')
        AWS_SECRET_ACCESS_KEY = credentials('aws-sec-key')
    }
    stages {
        stage('clone repo') {
            steps {
                git url: 'https://github.com/poojaganvir2607/hello_maven.git'
                sh 'ls -lrt'
            }
        }
        stage('compile code') {
            steps {
                dir('lcd') {
                    sh 'mvn clean package'
                    sh 'mvn test'
                    sh 'ls -lrt'
                }
                
            }
        }
    stage('Docker Login & Build Image') {
            steps {
                echo 'Building Docker image...'
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        cd lcd
                        aws ecr get-login-password --region us-east-1   | docker login --username AWS --password-stdin 747409585857.dkr.ecr.us-east-1.amazonaws.com
                        cp target/lcd-*-SNAPSHOT.jar lcd.jar
                        docker build -t "747409585857.dkr.ecr.us-east-1.amazonaws.com/lcd:v${BUILD_NUMBER}" .
                    '''
                }
            }
        }
        
    stage('push docker image') {
            steps {
                echo 'Pushing Docker image...'
                sh 'aws ecr --region us-east-1 describe-repositories --repository-names lcd || aws ecr create-repository --repository-name --region us-east-1 lcd'
                sh 'docker push "747409585857.dkr.ecr.us-east-1.amazonaws.com/lcd:v${BUILD_NUMBER}"'
            }
        }
    
        
        
    }
    
    
    
    
    
    
}


