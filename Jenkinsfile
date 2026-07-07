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

        stage('Push To Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhubcreds',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {

                    sh '''
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    docker tag notes-flask-app:latest $DOCKER_USERNAME/notes-flask-app:latest
                    docker push $DOCKER_USERNAME/notes-flask-app:latest
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker compose up -d --build flaskapp'
            }
        }

    }

    post {

        success {
            emailext(
                from: 'jaibardiya508@gmail.com',
                to: 'jaibardiya508@gmail.com',
                subject: 'Build Success - Notes App CI/CD',
                body: 'Build completed successfully and deployment finished.'
            )
        }

        failure {
            emailext(
                from: 'jaibardiya508@gmail.com',
                to: 'jaibardiya508@gmail.com',
                subject: 'Build Failed - Notes App CI/CD',
                body: 'The Jenkins build failed. Please check the console logs.'
            )
        }
    }
}
