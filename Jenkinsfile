pipeline{
    agent any
    tools{
        jdk 'java17'
        nodejs 'node16'
    }
   
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/kri-sh27/Zomato-Clone.git'
            }
        }
    
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
         stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker build -t zomato ."
                       sh "docker tag zomato krishnahogale/zomato:latest "
                       sh "docker push krishnahogale/zomato:latest "
                    }
                }
            }
        }
        stage('Deploy to container'){
     steps{
            sh 'docker run -d --name zomato -p 3000:3000 krishnahogale/zomato:latest'
          }
      }
    }
}