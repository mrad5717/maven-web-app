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
                git branch: '*/master', url: 'https://github.com/mrad5717/maven-web-app.git'
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
                echo 'Deploying to DEV environment...'
                sh '''
                    docker stop maven-web-app-dev || true
                    docker rm maven-web-app-dev || true
                    docker run -d --name maven-web-app-dev -p 9090:8080 maven-web-app:latest
                '''
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed!'
        }
        success {
            echo 'Pipeline succeeded! DEV environment running on port 9090'
        }
        failure {
            echo 'Pipeline failed! Check the logs.'
        }
    }
}
