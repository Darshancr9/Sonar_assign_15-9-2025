pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://184.73.14.110:9000'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          userRemoteConfigs: [[url: 'https://github.com/Darshancr9/Sonar_assign_15-9-2025.git']]])
            }
        }

        stage('SonarQube Scan') {
            steps {
                withCredentials([string(credentialsId: 'Sample_app', variable: 'SONAR_TOKEN')]) {
                    echo 'Running SonarQube analysis...'
                    sh """
                        mvn clean verify sonar:sonar \
                            -Dsonar.projectKey=main \
                            -Dsonar.host.url=${SONAR_HOST_URL} \
                            -Dsonar.token=${SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Build & Package') {
            steps {
                echo 'Building and packaging the project...'
                sh 'mvn clean package'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}

