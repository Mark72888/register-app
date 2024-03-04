pipeline {
	agent { label 'Jenkins-Agent' }
	tools {
		jdk 'Java17'
		maven 'Maven3'
	      }
	environment {
        REGISTRY_URL = 'https://hub.docker.com/repositories/mark72888'
        DOCKER_PASS = credentials('Mark.lalli@123')
        IMAGE_NAME = 'register-app-pipeline'
        IMAGE_TAG = 'register-app-pipeline-o1'
    }
	stages {
		stage("Cleanup Workspace"){
			steps {
			cleanWs()
		    }
		}
	
		stage("Checkout"){
			steps {
			git branch: 'main', credentialsId: 'github', url: 'https://github.com/Mark72888/register-app.git'
		    }
		}
		
		stage("build"){
			steps {
				sh "mvn clean package"
		    }
			
		}
		
		stage("Test"){
			steps {
				sh "mvn test"
		    }	
		}

		stage("SonarQube Analysis"){
			steps {
			script {
				withSonarQubeEnv(credentialsid: 'Jenkins-SonarQube-token') {
				sh "mvn sonar:sonar"
		  	 	 }	
			  }
		}
	}
		
		stage("Quality Gate"){
			steps {
			script {
				waitForQualityGate abortPipeline: false, credentialsid: 'Jenkins-SonarQube-token'
		   	 }	
		  }
	     }
     }
}
