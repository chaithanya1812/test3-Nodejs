// jobname=nodejs
/* This job based on Build with parameters
   you can select how many containers to start to acesses of this application
 */  
pipeline{
    agent {
        label 'NodejsJobs'
    }
    tools{
        nodejs 'NodeJS17.8.0'
    }
    parameters {
     choice choices: ['1', '2', '3'], description: 'How many container you want to start?', name: 'containers'
    }
    stages{
        stage('webhook'){
            steps{
                script{
                properties([pipelineTriggers([githubPush()])])
                }
            }
        }
        stage('git'){
            steps{
                git branch: 'FIRST', url: 'https://github.com/chaithanya1812/test3-Nodejs.git'
            }
        }
        stage('BUILD'){
            steps{
                script{
                nodejs(nodeJSInstallationName: 'NodeJS17.8.0'){
                    sh 'npm install'
                }
                
                }
            }
        }
        stage('Docker build & Docker login'){
            steps{
                withCredentials([string(credentialsId: 'ddockerloginn', variable: 'login')]) {
                    sh 'docker login -u chaitu1812 --password $login'
                }    
                sh 'docker build -t chaitu1812/test3-nodejs:latest .'    
                sh 'docker push chaitu1812/test3-nodejs:latest'
                sh 'docker rmi chaitu1812/test3-nodejs:latest'
                }
            }
            stage('To delete containers'){
                steps{
                    sh 'docker stop $(docker ps -q)'
                    sh 'docker rm -f $(docker ps -aq)'
                }
            }
            
            
        stage('container=1'){
            when {
                expression { "${params.containers}"  == '1'}
            }
        steps{
            sh 'docker run -d --name c1 -p 8091:3000 chaitu1812/test3-nodejs:latest'
        }    
        }
        stage('container=2'){
            when {
                expression { "${params.containers}"  == '2'}
            }
        steps{
            sh 'docker run -d --name c1 -p 8091:3000 chaitu1812/test3-nodejs:latest'
            sh 'docker run -d --name c2 -p 8092:3000 chaitu1812/test3-nodejs:latest'
        }    
    }
        stage('container=3'){
            when {
                expression { "${params.containers}"  == '3'}
            }
        steps{
            sh 'docker run -d --name c1 -p 8091:3000 chaitu1812/test3-nodejs:latest'
            sh 'docker run -d --name c2 -p 8092:3000 chaitu1812/test3-nodejs:latest'
            sh 'docker run -d --name c3 -p 8093:3000 chaitu1812/test3-nodejs:latest'
        }    
    }
    }            
}
