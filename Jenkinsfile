pipeline {
    agent any
    tools{
        maven : 'maven_3_5_0'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/hmhamsa17/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t hm17/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'docker-cred', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u hm17 -p ${dockerhubpwd}'

                    }
                   sh 'docker push hm17/devops-integration'
                }
            }
        }
        
    }
}
