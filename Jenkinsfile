pipeline {
    agent any
	  tools
    		{
      		 maven "maven"
   		 }
 stages {
      stage("Git clone"){
	steps{
		git credentialsId: 'github_id', url: 'https://github.com/ruthwikkulkarni/rock-paper-scissors.git'
			}
             }
	 stage("Maven Build"){
		steps{
			sh 'mvn --version'
			sh "mvn clean install"
                            }
	 		}
  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t mypythonapp:latest .' 
                sh 'docker tag mypythonapp ruthwikkulkarni/mypythonapp:latest'
                //sh 'docker tag mypythonapp ruthwikkulkarni/mypythonapp:$BUILD_NUMBER'
               
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        withDockerRegistry([ credentialsId: "dockerhub_id", url: "https://hub.docker.com/repository/docker/ruthwikkulkarni/mypythonapp" ]) {
          sh  'docker push ruthwikkulkarni/mypythonapp:latest'
        //   sh  'docker push ruthwikkulkarni/mypythonapp:$BUILD_NUMBER' 
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
                sh "docker -H ssh://jenkins@3.140.216.35 run -d -p 8003:5000 ruthwikkulkarni/mypythonapp"
 
            }
        }
    }
	}
