pipeline {
    agent any

    environment {
        // Environment variables for easier maintenance
        GIT_REPO_URL = 'https://github.com/saakanbi/NumberGuessGame.git'
        GIT_BRANCH = 'dev'
        MAVEN_HOME = tool name: 'Maven', type: 'ToolType'  // Use 'Maven' tool defined in Jenkins
        BUILD_DIR = 'target'  // The directory where Maven outputs the WAR file
    }

    options {
        // Timeout the entire pipeline after 1 hour
        timeout(time: 1, unit: 'HOURS')
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // If repo is private, add credentialsId
                    echo 'Checking out code from GitHub repository...'
                    git branch: GIT_BRANCH, url: GIT_REPO_URL
                }
            }
        }

        stage('Build') {
            steps {
                // Ensure the steps are executed inside a node block to provide necessary workspace context
                node {
                    echo 'Building the project...'
                    // Cache Maven dependencies to speed up subsequent builds
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
                    // Place deployment scripts here, e.g., copying files or running deployment scripts
                    // sh './deploy.sh'  // Uncomment and modify if needed
                }
            }
        }
    }

    post {
        success {
            // Archive the WAR file as an artifact after the build completes
            node {
                archiveArtifacts artifacts: "${BUILD_DIR}/*.war", allowEmptyArchive: true
            }
            echo '✅ Build and Deployment Successful!'
            // Optional: You could also trigger notifications (e.g., email or Slack) here
        }

        failure {
            node {
                echo '❌ Build Failed! Please check the logs for issues.'
            }
            // Send notifications to inform stakeholders about the failure (could be via email, Slack, etc.)
            // notifyFailure()
        }

        always {
            node {
                // Always run, regardless of success or failure
                cleanWs()  // Clean workspace to free up space
                echo 'Cleanup complete. Workspace cleaned.'
            }
        }
    }
}
