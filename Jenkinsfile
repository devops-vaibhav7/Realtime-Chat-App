pipeline{
    agent any
    
    stages{
        stage("Code Clone"){
            steps{
                echo "Code Clone Stage"
                git url: "https://github.com/devops-vaibhav7/Realtime-Chat-App", branch: "main"
            }
        }
        stage("Code Build & Test"){
            steps{
                echo "Code Build Stage"
                sh "cd backend && docker build -t chat-app-backend . && cd .."
                sh "cd frontend && docker build -t chat-app-frontend . && cd .."
            }
        }
        stage("Push To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"DockerCreds",
                    usernameVariable:"DockerUser", 
                    passwordVariable:"DockerPass")]){
                sh 'echo $DockerPass | docker login -u $DockerUser --password-stdin'
                sh "docker image tag chat-app-frontend:latest ${env.DockerUser}/chat-app-frontend:latest"
                sh "docker push ${env.DockerUser}/chat-app-frontend:latest"
                sh "docker image tag chat-app-backend:latest ${env.DockerUser}/chat-app-backend:latest"
                sh "docker push ${env.DockerUser}/chat-app-backend:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d --build"
            }
        }
    }
}