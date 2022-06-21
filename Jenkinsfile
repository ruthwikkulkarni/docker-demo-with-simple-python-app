pipeline {
    agent any
	  tools
    		{
      		 maven 'maven' 
                   jdk 'jdk'
   		 }
 stages {
      stage("Git clone"){
	steps{
		git credentialsId: 'github_id', url: 'https://github.com/ruthwikkulkarni/rock-paper-scissors.git'
			}
             }
	 stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }
	 stage("Maven Build"){
		steps{
			sh 'mvn --version'
			sh "mvn clean install"
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
