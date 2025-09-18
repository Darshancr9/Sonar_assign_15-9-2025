pipeline {
    agent any

    // Environment variables
    environment {
        SONAR_HOST_URL = 'http://184.73.14.110:9000'       // SonarQube URL
        SONAR_AUTH_TOKEN = credentials('sonarqube-token')  // Jenkins credential ID for SonarQube token
        BRANCH_NAME = 'main'                                // Hardcoded branch
    }

    stages {
        // --------------------------
        stage('Checkout Code') {
            steps {
                script {
                    echo "Checking out branch: ${env.BRANCH_NAME}"
                    git branch: "${env.BRANCH_NAME}",
                        url: 'https://github.com/Darshancr9/Sonar_assign_15-9-2025.git'
                }
            }
        }

        // --------------------------
        stage('SonarQube Scan') {
            steps {
                script {
                    echo "Running SonarQube analysis..."
                    sh """
                        mvn clean verify sonar:sonar \
                          -Dsonar.projectKey=${env.BRANCH_NAME} \
                          -Dsonar.host.url=${SONAR_HOST_URL} \
                          -Dsonar.login=${SONAR_AUTH_TOKEN}
                    """
                }
            }
        }

        // --------------------------
        stage('Build & Package') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                script {
                    echo "Building the project and generating artifacts..."
                    sh """
                        mvn clean package
                        mkdir -p artifacts
                        cp target/*.jar artifacts/
                    """
                }
            }
            post {
                success {
                    archiveArtifacts artifacts: 'artifacts/*.jar', fingerprint: true
                    echo "Artifacts archived successfully."
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check logs for details."
        }
    }
}
