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
        
/*
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
	stage('Deploy QA') {
            steps {
                echo 'Deploying to QA environment...'
                sh '''
                    docker stop maven-web-app-qa || true
                    docker rm maven-web-app-qa || true
                    docker run -d --name maven-web-app-qa -p 9091:8080 maven-web-app:latest
                '''
            }
        }
*/
	stage('Deploy DEV') {
    steps {
        echo 'Deploying to Kubernetes DEV environment...'
        sh '''
            # Create or update deployment
            kubectl create deployment maven-web-app-dev --image=maven-web-app:latest --dry-run=client -o yaml | kubectl apply -f -
            
            # Expose as NodePort service
            kubectl expose deployment maven-web-app-dev --port=8080 --target-port=8080 --type=NodePort --dry-run=client -o yaml | kubectl apply -f -
            
            # Show status
            kubectl get pods
            kubectl get services
        '''
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
