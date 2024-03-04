pipeline {
	agent { label 'Jenkins-Agent' }
	tools {
		jdk 'Java17'
		maven 'Maven3'
	      }
	environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "mark72888"
            DOCKER_PASS = 'Mark.lalli@123'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
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

		stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
               }
            }
         }
     }
  }
}
