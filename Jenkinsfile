pipeline {
    agent any
    tools{
        maven 'maven_3_9_3'
    }
     stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/hmhamsa17/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
	stage('Static Code Analysis') {
      environment {
        SONAR_URL = "http://13.232.19.173/:9000"
      }
      steps {
        withCredentials([string(credentialsId: 'sonarqube-scanner', variable: 'Secret')]) {
          sh 'mvn sonar:sonar -Dsonar.login=$Secret -Dsonar.host.url=${SONAR_URL}'
        }
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
                   withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u hm17 -p ${dockerhubpwd}'

                    }
                   sh 'docker push hm17/devops-integration'
                }
            }
        }
        
    }
}
