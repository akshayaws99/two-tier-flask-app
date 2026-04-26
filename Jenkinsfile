pipeline{
    agent any;
    stages {
        stage('code checkout'){
            steps{
                echo 'code checkout'
                git url: "https://github.com/akshayaws99/two-tier-flask-app.git" , branch: "master" 
            }
        }    
          stage('build'){
            steps{
                echo 'code build'
                sh " docker build -t flask-app ."
            }
        }
          stage('test'){
             steps{
                 echo 'code test'
                 
            }
        }
           stage('code push'){
             steps{
                 
                 withCredentials([usernamePassword(
                     credentialsId:"dockerHubCreds",
                     passwordVariable: "dockerHubPass",
                     usernameVariable: "dockerHubUser"
                 )]){
                 sh "echo ${env.dockerHubPass} | docker login -u ${env.dockerHubUser} --password-stdin"
                 sh "docker image tag flask-app ${env.dockerHubUser}/two-tier-flaskapp:latest"
                 sh "docker image tag flask-app ${env.dockerHubUser}/two-tier-flaskapp:${env.BUILD_NUMBER} "
                 sh "docker push ${env.dockerHubUser}/two-tier-flaskapp:latest"
                 sh "docker push ${env.dockerHubUser}/two-tier-flaskapp:${env.BUILD_NUMBER}"
                
                 }
            }
        }
          stage('deploy'){
              steps{
                  sh "docker compose up -d  --build flask-app"
              }
          }
    }
}
