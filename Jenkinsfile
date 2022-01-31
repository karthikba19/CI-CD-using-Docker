pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/devops4solutions/CI-CD-using-Docker.git'
             
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
                sh 'docker tag samplewebapp karthikba/samplewebapp:latest'
                //sh 'docker tag samplewebapp karthikba/samplewebapp:$BUILD_NUMBER'
               
          }
        }
     
   stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockerhub", url: "" ]) {
          sh  'docker push karthikba/samplewebapp:latest'
        //  sh  'docker push karthikba/samplewebapp:$BUILD_NUMBER' 
        }
                  
          }
        }
 
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8080 karthikba/samplewebapp"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@172.31.37.44 run -d -p 8003:8080 karthikba/samplewebapp"
 
            }
        }
    }
	}
    
