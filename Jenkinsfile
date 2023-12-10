pipeline {
    agent any
	
	
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/akshu20791/CI-CD-using-Docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t samplewebapp:latest .' 
                sh 'docker tag samplewebapp akshu20791/samplewebapp:latest'
                //sh 'docker tag samplewebapp akshu20791/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockercred", url: "" ]) {
          sh  'docker push akshu20791/samplewebapp:latest'
        //  sh  'docker push akshu20791/samplewebapp:$BUILD_NUMBER' 
        }
                  
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8080 akshu20791/samplewebapp"
	        sh "kubectl -f pod1.yml"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                script {
                    def dockerCmd = 'docker run -d -p 8003:8080 akshu20791/samplewebapp'
                    sshagent(['sshkeypair']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.20.232 ${dockerCmd}"
                    }
                }
            }
        }
    }
	}
    
