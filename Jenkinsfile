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
	
     
	 	stage(' stage('SonarQube analysis') {
    		def scannerHome = tool 'sonarqube';
	   	 withSonarQubeEnv('sonarqube') {
      		sh "${scannerHome}/bin/sonar-scanner \
      		-D sonar.login=admin \
      		-D sonar.password=admin \
      		-D sonar.projectKey=sonarqubetest \
      		-D sonar.exclusions=vendor/**,resources/**,**/*.java \
     		 -D sonar.host.url=http://192.168.1XX.XX:9000/"
    		}
    	 }
	
	       Build docker image'){
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
