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
	
     
	 stage('SonarQube analysis') {
    		 scannerHome = tool 'sonarqube';
	   	 withSonarQubeEnv('sonarqube') {
      		sh "${scannerHome}/bin/sonar-scanner \
      		-D sonar.login=admin \
      		-D sonar.password=admin123 \
      		-D sonar.projectKey=demo1 \
      		-D sonar.exclusions=vendor/**,resources/**,**/*.java \
     		 -D sonar.host.url=http://13.232.19.173:9000/"
    		}
    	 }
	stage("Quality Gate") {
  steps {
    timeout(time: 1, unit: 'MINUTES') {
        waitForQualityGate abortPipeline: true
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
