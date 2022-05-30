properties([pipelineTriggers([githubPush()])])
pipeline {
    
    agent any
    
    environment{
        dockerhub=credentials('dockerhub')
    }
    
    stages{
        
        stage('Cloning the git repo'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Chakravarthy7102/Node-Proxy-Server.git']]])
            }
        }
        
        stage('BUILD THE IMAGE'){
            steps{
                sh 'ls -lah'
                sh 'groups'
                sh 'docker build -t node:v1 .'
            }
        }
        
        stage("TAG THE IMAGE"){
            steps{
                sh 'docker tag node:v1 chakravarthy712/node:v1'
                //login to the dockerhub account
                sh 'echo $dockerhub_USR $DOCKER_PSW'
                // sh 'docker login'
            }
        }
        
        stage("PUSH IAMGE TO THE DOCKER HUB"){
            steps{
                sh 'echo $DOCKER_PSW | docker login -u $dockerhub_USR --password-stdin'
                sh 'docker push chakravarthy712/node:v1'
            }
        }
        
        stage("START CONTEINET"){
            steps{
                sh "docker rm -f node | true"
                sh 'docker run --name node -d -p 3000:4000 chakravarthy712/node:v1'
            }
        }
    }
}
