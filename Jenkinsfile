pipeline {
    agent any
	
@Library('shared-library@master') _ //master or whatever branch
	
 stages {
      stage('checkout') {
           steps {             
                git branch: 'master', url: 'https://github.com/bhavanaht5/CI-CD-using-Docker.git'         
          }
        }
	 stage('Execute Maven') {
           steps {             
                sh 'mvn package'             
          }
        }
	 
	 stage ('Check logs') {
                    steps {
                        filterLogs ('WARNING', 5)
                    }
                }
        
     stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t samplewebapp:latest .' 
                sh 'docker tag samplewebapp bhavanaht5/samplewebapp:latest'                               
          }
        }
     
     stage('Publish image to Docker Hub') {         
            steps {
        withDockerRegistry([ credentialsId: "bhavanaht5", url: "" ]) { 
		sh  'docker push bhavanaht5/samplewebapp:latest'
            }                 
          }
        }
	 
     stage('Stop and Remove Previous build containers') {
		 steps
		 {
			sh "docker ps -f name=tomcat_container -q | xargs --no-run-if-empty docker container stop"
			sh "docker ps -a -f status=exited -q | xargs --no-run-if-empty docker rm"
		 }
	 }
     /*stage('Docker previous build images removing') {
		 steps
		 {
			sh "docker images bhavanaht5/samplewebapp -q | xargs --no-run-if-empty docker rmi --force"
	
		 }
	 } */
	 
      stage('Run Docker container on Jenkins Agent') {          
            steps 
		{
                sh 'docker run -it -d -p 8003:8080 --name=tomcat_container bhavanaht5/samplewebapp'
            }
        }
 	// use url to ping: http://PublicIPAddpress:8003//LoginWebApp-1/  
 }
	}
