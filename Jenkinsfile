pipeline {
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    stages{
        stage("Cleanup Workspace"){
                steps {
                cleanWs()
                }
        }

        stage("Checkout from SCM"){
                steps {
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/Mark72888/register-app.git'
                }
        }

        stage("build app"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("test app"){
           steps {
                 sh "mvn test"
           }
       }
}
