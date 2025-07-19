pipeline {
    environment {
        DOCKERHUB_CREDENTIALS = credentials("dockerhub")
    }

    agent {
        label 'K-M'
    }

    triggers {
        githubPush() // Automatically trigger build on GitHub push
    }

    stages {
        stage('Clone GitHub Repo') {
            steps {
                git url: 'https://github.com/firdosneha/Website-PRT-ORG', branch: 'main'
            }
        }

        stage('Docker Build & Push') {
            steps {
                sh '''
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                docker build -t firdosneha/prt-task .
                docker push firdosneha/prt-task
                '''
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s-deployment.yml'
                sh 'kubectl apply -f k8s-service.yml'
            }
        }
    }
}
