pipeline {
    agent any
    
    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/amanmullal/django-todo-cicd.git", branch: "develop"
            }
        }
        
        stage("Build & Test") {
            steps {
                sh "docker build . -t django-app"
            }
        }
        
        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubpass", usernameVariable: "dockerHubuser")]) {
                    sh "docker login -u $dockerHubuser -p $dockerHubpass"
                    sh "docker tag django-app $dockerHubuser/django-app:latest"
                    sh "docker push $dockerHubuser/django-app:latest"
                }
            }
        }
        
        stage("Deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d --no-deps --build web"
                
            }
        }
    }
}
