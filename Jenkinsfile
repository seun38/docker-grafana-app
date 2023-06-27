pipeline {

    tools{

        maven 'maven3.9.2'
    }
    agent any

    environment {
        registry = "757750585556.dkr.ecr.us-east-1.amazonaws.com/liontechclass20"
        DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred-raja')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Lion-Technology-Solutions/grafana-docker-pipeline.git']])
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    sh 'docker build -t bharathirajatut/nodeapp  .'
                    dockerImage = docker.build registry
                    dockerImage.tag("$BUILD_NUMBER")
                }
            }
        }

        stage('dockerhublogin'){
            steps{
               sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin' 
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 757750585556.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    sh 'docker push 757750585556.dkr.ecr.us-east-1.amazonaws.com/liontechclass20:$BUILD_NUMBER'
                    sh 'docker push bharathirajatut/nodeapp:$BUILD_NUMBER'
                }
            }
        }

 
        }    
    }
