pipeline {
    agent any
    
    tools {
        // Using the Maven and JDK versions configured in Jenkins
        maven 'M3'
        jdk 'JDK17'
    }
    
    environment {
        // Docker registry credentials (if needed)
        // DOCKER_REGISTRY = 'your-registry-url'
        // DOCKER_IMAGE = 'springdemo'
        // DOCKER_TAG = "${env.BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'mvn clean compile'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'mvn test'
            }
            post {
                always {
                    // Archive test results even if tests fail
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Package') {
            steps {
                echo 'Packaging the application...'
                sh 'mvn package -DskipTests'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
        
        stage('Docker Build') {
            steps {
                echo 'Building Docker image...'
                script {
                    // Build Docker image
                    sh 'docker build -t springdemo:${BUILD_NUMBER} .'
                }
            }
        }
        
        stage('Docker Push') {
            // This stage is commented out by default
            // Uncomment and configure when you want to push to registry
            steps {
                echo 'Pushing Docker image to registry...'
                script {
                    // Example for pushing to Docker Hub or private registry:
                    // sh 'docker tag springdemo:${BUILD_NUMBER} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG}'
                    // sh 'docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG}'
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}