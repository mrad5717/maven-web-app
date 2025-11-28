pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'JDK-11'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Getting source code from Git...'
            }
        }

        stage('Maven Build') {
            steps {
                echo 'Building with Maven...'
                sh 'mvn clean package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                echo 'Building and pushing Docker image...'
                sh '''
                    docker build -t mururadh/maven-web-app:latest .
                    echo "Abhi@72485" | docker login -u mururadh --password-stdin
                    docker push mururadh/maven-web-app:latest
                '''
            }
        }

        stage('Deploy DEV') {
            steps {
                echo 'Deploying to Kubernetes DEV environment...'
                sh '''
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
                '''
            }
        }

        stage('Deploy QA') {
            steps {
                echo 'Deploying to Kubernetes QA environment...'
                sh '''
                    kubectl apply -f deployment-qa.yaml
                    kubectl apply -f service-qa.yaml
                '''
            }
        }

        stage('Verify Deployments') {
            steps {
                echo 'Checking deployment status...'
                sh '''
                    kubectl get pods
                    kubectl get services
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed!'
        }
        success {
            echo 'Pipeline succeeded! DEV and QA environments deployed.'
        }
        failure {
            echo 'Pipeline failed! Check the logs.'
        }
    }
}
