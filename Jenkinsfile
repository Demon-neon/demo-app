pipeline {
    agent any

    environment {
        ECR = "<AWS_ACCOUNT_ID>.dkr.ecr.<REGION>.amazonaws.com/demo-app"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<username>/demo-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t $ECR:${env.BUILD_NUMBER} .
                docker push $ECR:${env.BUILD_NUMBER}
                """
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh """
                kubectl set image deployment/demo-app \
                demo-app=$ECR:${env.BUILD_NUMBER} --record || \
                kubectl apply -f k8s-deployment.yaml
                """
            }
        }
    }
}
