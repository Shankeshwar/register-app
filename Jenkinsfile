pipeline {
    agent { label 'Jenkins-Agent' }
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
                    git branch: 'main', credentialsId: 'github', url: 'https://github.com/Shankeshwar/register-app.git'
                }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }

       }

       stage("Test Application"){
           steps {
                 sh "mvn test"
           }
       }

       stage("SonarQube Analysis"){
           steps {
                   script {
                        withSonarQubeEnv(credentialsId: 'Jenkins-SonarQube-Token') {
                        sh "mvn sonar:sonar"
                        }
                   }
           }
        }
	stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: true, credentialsId: 'Jenkins-SonarQube-Token'
                }
            }

        }
	}
}	
