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
//                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        docker build -t mururadh/maven-web-app:latest .
                        echo Äbhi@72485 | docker login -u mururadh --password-stdin
                        docker push mururadh/maven-web-app:latest
                    '''
                }
            }
        }

        stage('Deploy DEV') {
            steps {
                echo 'Deploying to Kubernetes DEV environment...'
                sh '''
                    kubectl apply -f deployment.yaml
                    kubectl apply -f service.yaml
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

