pipeline {
    agent any

    stages {

        stage('Code Clone') {
            steps {
                git url: 'https://github.com/Jai-Bardiya/notes-app-devops.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t notes-flask-app:latest .'
            }
        }

        stage('Test') {
            steps {
                echo "Running Tests..."
            }
        }
        stage("Push TO Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerhubcreds",
                    passwordVariable: "dockerhubpass",
                    usernameVariable: "dockerhubusername")]){
                sh "docker login -u ${env.dockerhubusername} -p ${env.dockerhubpass}"
                sh "docker image tag notes-flask-app ${env.dockerhubusername}/notes-flask-app"
                sh "docker push ${env.dockerhubusername}/notes-flask-app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker compose up -d --build flaskapp'
            }
        }

    }
}
