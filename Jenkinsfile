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

        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t maven-web-app:latest .'
            }
        }

        stage('Deploy DEV') {
            steps {
                echo 'Deploying to Kubernetes DEV environment...'
                sh '''
                    kubectl create deployment maven-web-app-dev --image=maven-web-app:latest --dry-run=client -o yaml | kubectl apply -f -
                    kubectl expose deployment maven-web-app-dev --port=8080 --target-port=8080 --type=NodePort --dry-run=client -o yaml | kubectl apply -f -
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
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed! Check the logs.'
        }
    }
}

