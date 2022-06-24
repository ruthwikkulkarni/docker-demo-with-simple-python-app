pipeline {
    agent any
	
	environment {
        	//once you sign up for Docker hub, use that user_id here
        registry = "ruthwikkulkarni/mypythonapp"
        	//- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = 'dockerhub_id'
        dockerImage = ''
	}
	 	stages {
     	 stage("Git clone"){
		steps{
		git credentialsId: 'github_id', url: 'https://github.com/ruthwikkulkarni/docker-demo-with-simple-python-app.git'
			}
             }
	  
	 stage('Building image') {
		  // Building Docker images
     		 steps{
        		script {
          			dockerImage = docker.build registry
        			}
     			 }
   		 }
     
   		// Uploading Docker images into Docker Hub
   	 stage('Upload Image') {
     		steps{    
        		script {
           		 docker.withRegistry( '', registryCredential ) {
            		 dockerImage.push()
           		 }
        		}
      		}
   	 }
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:5000 ruthwikkulkarni/mypythonapp"
 
            }
        }
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@3.144.132.28 run -d -p 8003:5000 ruthwikkulkarni/mypythonapp"
 
            }
        }
    }
	}
