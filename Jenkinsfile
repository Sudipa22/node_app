@Library('sudipa')_
 
pipeline {
    environment { 
        registry = "sudipa2020/sample-nodeapp" 
        registryCredential = 'docker-id' 
        dockerImage = '' 
    }
    agent any
    stages {
        stage('Initialize')
        {
            steps {
                script {
        def dockerHome = tool 'docker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
                }
            }
        }
 
        stage('Checkout') {
            steps{
       
             //git credentialsId: 'bitbucket_Url', url: 'http://rig@18.224.68.30:7990/scm/dem/app.git'
              //gitcheckout("main","https://github.com/Sudipa22/node_app.git/")
               git 'https://github.com/nitinkundu/node_app.git'
              }
            }
            
            stage('Building our image')
        {
            steps
            {
                sh 'docker --version'
                script
                {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
            
            
            stage('Push our image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
            }
            stage('Pull our image'){
            steps{
                script{
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.pull()
                   
                }
            }
            sh  'docker images'
        }
    }
    stage('Run Image'){
           steps{
               sh ''' 
               if [ $(docker ps -qf "name=nodejs_app") ]
                then
                echo "from if block"
                docker kill nodejs_app && docker rm nodejs_app
                docker run -d -p 3456:8080 --name nodejs_app "${registry}":"${BUILD_NUMBER}"
                docker ps
               else
                echo "from else block"
                docker run -d -p 3456:8080 --name nodejs_app "${registry}":"${BUILD_NUMBER}"
                docker ps
                fi
               '''
           }
       }   
     
            
            
            
    }
}
