pipeline {
	agent { label 'Jenkins-Agent' }
	tools {
		jdk 'Java17'
		maven 'Maven3'
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
				sh "mvn sonr:sonar"
		   	 }	
		  }
	     }
	}
    }
}
