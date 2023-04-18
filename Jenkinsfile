pipeline {
    agent { label 'agent-1' }
    
    stages{
        stage('Code'){
            steps {
                git url: 'https://github.com/ravikant1983/node-todo-cicd.git', branch: 'master'
            }
        }
        stage('Build and Test'){
            steps {
                sh 'docker build . -t rkg1983/node-todo-app:latest' 
            }
        }
        stage('Login and Push Image'){
            steps {
                echo 'logging in to docker hub and pushing image..'
                withCredentials([usernamePassword(credentialsId:'dockerHub',passwordVariable:'dockerHubPassword', usernameVariable:'dockerHubUser')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker push rkg1983/node-todo-app:latest"
                }
            }
        }
        stage('Deploy'){
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
