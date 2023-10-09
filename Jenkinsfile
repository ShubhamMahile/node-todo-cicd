pipeline {
    agent any
    
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/ShubhamMahile/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build & Test"){
            steps{
                bat "docker build . -t node-app-test-new"
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    bat "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    bat "docker tag node-app-test-new ${env.dockerHubUser}/node-app-test-new:latest"
                    bat "docker push ${env.dockerHubUser}/node-app-test-new:latest" 
                }
            }
        }
        stage("Deploy"){
            steps{
                bat "docker-compose down && docker-compose up -d"
            }
        }
    }
}
