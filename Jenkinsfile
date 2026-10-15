pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'deepasri111/student-placement-portal'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                echo 'Student Placement Portal source code downloaded.'
            }
        }

        stage('Build Docker Image') {
            steps {
               bat 'docker build -t deepasri111/student-placement-portal:%BUILD_NUMBER% -t deepasri111/student-placement-portal:latest .'            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKERHUB_USERNAME',
                    passwordVariable: 'DOCKERHUB_PASSWORD'
                )]) {
                    bat '''
                        echo %DOCKERHUB_PASSWORD% | docker login -u %DOCKERHUB_USERNAME% --password-stdin
                        docker push deepasri111/student-placement-portal:%BUILD_NUMBER%
                        docker push deepasri111/student-placement-portal:latest
                        docker logout
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Docker Hub push completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check the Jenkins console output.'
        }
    }
}