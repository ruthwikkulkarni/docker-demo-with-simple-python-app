pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/ruthwikkulkarni/docker-demo-with-simple-python-app.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
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
        withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
          sh  'docker push ruthwikkulkarni/mypythonapp:latest'
        //  sh  'docker push ruthwikkulkarni/mypythonapp:$BUILD_NUMBER' 
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
