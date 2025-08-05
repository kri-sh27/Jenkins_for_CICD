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
                git branch: 'main', url: 'https://github.com/kri-sh27/Jenkins_for_CICD.git'
            }
        }
    
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }

          stage('Run tests') {
            steps {
                // Run tests
                sh 'npm test'
            }
        }
         stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker build -t zomatoapp ."
                       sh "docker tag zomatoapp krishnahogale/zomatoapp:latest "
                       sh "docker push krishnahogale/zomatoapp:latest "
                    }
                }
            }
        }
    //     stage('Deploy to container'){
    //  steps{
    //         sh 'docker run -d --name zomatoapp -p 3000:3000 krishnahogale/zomatoapp:latest'
    //       }
    //   }
    stage('Deploy to Container') {
    steps {
        script {
            // Stop and remove existing container if it exists
            sh '''
            if [ $(docker ps -aq -f name=zomatoapp) ]; then
              docker stop zomatoapp || true
              docker rm zomatoapp || true
            fi
            docker run -d --name zomatoapp -p 3000:3000 krishnahogale/zomatoapp:latest
            '''
        }
    }
}
    }
}