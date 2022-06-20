pipeline {
    agent any 
    environment {
				//once you sign up for Docker hub, use that user_id here
        registry = "ruthwikkulkarni/mypythonapp"
		
				//- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = 'DockerHub_ID'
        dockerImage = ''
    }
    
    stages {
        stage('Cloning Git') {
            steps {
                git credentialsId: 'github', url: 'https://github.com/ruthwikkulkarni/docker-demo-with-simple-python-app.git'       
            }
        }
    
    // Building Docker images
    stage('Building image') {
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
    
     // Stopping Docker containers for cleaner Docker run
     stage('docker stop container') {
         steps {
            sh 'docker ps -f name=mypythonappContainer -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=mypythonappContainer -q | xargs -r docker container rm'
         }
       }
    
    
    // Running Docker container, make sure port 8096 is opened in 
    stage('Docker Run') {
     steps{
         script {
            dockerImage.run("-p 8096:5000 --rm --name mypythonappContainer")
         }
      }
    }
  }
}
