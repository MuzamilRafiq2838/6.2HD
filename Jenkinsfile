pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'Distinction'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'Deakin', url: 'https://github.com/MuzamilRafiq2838/6.2HD.git'
            }
        }
        
        stage('Build') {
            steps {
                script {
                    sh 'echo "Building the project..."'
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh 'echo "Running tests..."'
                    sh 'docker run --rm $DOCKER_IMAGE ./run_tests.sh'
                    junit '**/target/test-results/*.xml'
                }
            }
        }

        stage('Code Quality Analysis') {
            steps {
                script {
                    sh 'echo "Running ESLint analysis..."'
                    sh 'docker run --rm $DOCKER_IMAGE npm run lint'
                }
            }
        }

        stage('Deploy to Test Environment') {
            steps {
                script {
                    sh 'echo "Deploying to Test Environment..."'
                    sh 'docker-compose -f docker-compose.test.yml up -d'
                }
            }
        }

        stage('Release to Production') {
            steps {
                script {
                    sh 'echo "Releasing to Production..."'
                    sh 'docker-compose -f docker-compose.prod.yml up -d'
                }
            }
        }

        stage('Monitoring and Alerting') {
            steps {
                script {
                    sh 'echo "Setting up monitoring and alerting..."'
                    // Add your monitoring and alerting setup commands here
                }
            }
        }
    }

    post {
        success {
            script {
                sh 'echo "Pipeline succeeded!"'
            }
        }

        failure {
            script {
                sh 'echo "Pipeline failed. Sending alerts..."'
                // Add alerting commands here (e.g., notify via email or monitoring tool)
            }
        }
    }
}
