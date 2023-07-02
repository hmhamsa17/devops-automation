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
         
        steps{
                      script{
                      withSonarQubeEnv('sonarqube') { 
                      sh "mvn sonar:sonar"
                       }
                      timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
		    sh "mvn clean install"
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
