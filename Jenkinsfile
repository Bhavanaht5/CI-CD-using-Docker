pipeline {
    agent any
	
	
 stages {
      stage('Checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/Bhavanaht5/CI-CD-using-Docker.git'
             
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
                sh 'docker tag samplewebapp bhavanaht5/samplewebapp:$BUILD_NUMBER'
                //sh 'docker tag samplewebapp bhavanaht5/samplewebapp:$BUILD_NUMBER'              
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
                withDockerRegistry([ credentialsId: "bhavanaht5", url: "" ]) {
     		sh  'docker push bhavanaht5/samplewebapp:$BUILD_NUMBERt'
                //  sh  'docker push bhavanaht5/samplewebapp:$BUILD_NUMBER' 
        }                 
       }
      }
     
      stage('Run Docker container on Jenkins Agent') {            
            steps 
        	{
                sh "docker run -d -p 8003:8080 bhavanaht5/samplewebapp"
            }
        }
 
 	// use url to ping: http://PublicIPAddpress:8003//LoginWebApp-1/  
 }
}
    
