pipeline {
    agent any

    stages {
        stage('Checkout') { 
            steps {
                git branch: 'dev', url: 'https://github.com/saakanbi/NumberGuessGame.git'
            }
        }

        stage('Build') { 
            steps {
                script {
                    sh 'mvn clean package' 
                }
            }
        }

        stage('Test') { 
            steps {
                sh 'mvn test' 
                sh 'ls -la target'  // Check what files are in the target directory for debugging
            }
        }

        stage('Deploy') { 
            steps {
                script {
                    try {
                        echo 'Deploying'
                        // Your deploy command, e.g., 'sh deploy.sh'
                    } catch (Exception e) {
                        echo "Deployment failed: ${e.getMessage()}"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
    }

    post { 
        success {
            echo 'Build and Deployment Successful'
        }
        failure {
            echo 'Build or Deployment failed'
        }

      
        }
    }
