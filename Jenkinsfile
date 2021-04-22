pipeline {
    agent any
	environment{
          CONTAINER_NAME = ''
}
	
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
                sh 'docker build -t bhavanaht5/samplewebapp .' 
                sh 'docker tag samplewebapp bhavanaht5/samplewebapp:$BUILD_NUMBER'
                //sh 'docker tag samplewebapp bhavanaht5/samplewebapp:$BUILD_NUMBER'              
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
                withDockerRegistry([ credentialsId: "bhavanaht5", url: "" ]) {
     		sh  'docker push bhavanaht5/samplewebapp:$BUILD_NUMBER'
                //  sh  'docker push bhavanaht5/samplewebapp:$BUILD_NUMBER' 
        }                 
       }
      }
     
	 stage('Stop and Remove Previous build containers') {
		 steps
		 {
                script{
if (env.CONTAINER_NAME == 'tomcat_container'){
            sh "docker ps -f name=tomcat_deploy -q | xargs --no-run-if-empty docker container stop"
            sh "docker ps -a -f status=exited -q | xargs --no-run-if-empty docker rm"
     }
}			
		 }
	 }
	 
      stage('Run Docker container on Jenkins Agent') {            
            steps 
        	{
                sh "docker run -d -p 8003:8080 --name=tomcat_container bhavanaht5/samplewebapp"
	        env.CONTAINER_NAME = 'tomcat_container'
            }
        }
 
 	// use url to ping: http://PublicIPAddpress:8003//LoginWebApp-1/  
 }
}
    
