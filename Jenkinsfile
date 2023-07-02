pipeline {
    agent any
    tools{
        maven 'maven_3_9_3'
    }
  environment {
        scannerHome = tool 'SonarQubeScanner'
    }
     stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/hmhamsa17/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
	stage('Sonarqube') {
		steps {
        		withSonarQubeEnv('sonarqube-scanner') {
	 		sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=cicd1 \ -Dsonar.java.binaries=. \ -Dsonar.projectKey=cicd1'''
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
