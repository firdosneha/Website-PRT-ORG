pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'your-dockerhub-username/prt-website:latest'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-github-username/Website-PRT-ORG.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yml'
                sh 'kubectl apply -f k8s-service.yml'
            }
        }
    }

    triggers {
        githubPush()
    }
}
