pipeline {
    agent any

    environment {
        GIT_REPO_URL = 'https://github.com/saakanbi/NumberGuessGame.git'
        GIT_BRANCH = 'main'
        MAVEN_HOME = tool name: 'Maven', type: 'ToolType'  // Correct reference to Maven tool configured in Jenkins
        BUILD_DIR = 'target'  // The directory where Maven outputs the WAR file
    }

    options {
        timeout(time: 1, unit: 'HOURS')
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    echo 'Checking out code from GitHub repository...'
                    git branch: GIT_BRANCH, url: GIT_REPO_URL
                }
            }
        }

        stage('Build') {
            steps {
                node {
                    echo 'Building the project...'
                    // Ensure Maven is used to build the project
                    sh "'${MAVEN_HOME}/bin/mvn' clean install -DskipTests"
                }
            }
        }

        stage('Test') {
            steps {
                node {
                    echo 'Running tests...'
                    // Run tests only after build is successful
                    sh "'${MAVEN_HOME}/bin/mvn' test"
                }
            }
        }

        stage('Deploy') {
            steps {
                node {
                    echo 'Deploying Application...'
                    // Place deployment scripts here
                    // sh './deploy.sh'  // Uncomment and modify if needed
                }
            }
        }
    }

    post {
        success {
            node {
                archiveArtifacts artifacts: "${BUILD_DIR}/*.war", allowEmptyArchive: true
            }
            echo '✅ Build and Deployment Successful!'
        }

        failure {
            node {
                echo '❌ Build Failed! Please check the logs for issues.'
            }
        }

        always {
            node {
                cleanWs()
                echo 'Cleanup complete. Workspace cleaned.'
            }
        }
    }
}
