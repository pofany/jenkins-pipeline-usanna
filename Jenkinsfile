pipeline {
    agent any
    triggers {
        pollSCM('H/2 * * * *')
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t usapp:latest .'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'docker run --rm usapp:latest'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Pushing Docker image to DockerHub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker tag usapp:latest $DOCKER_USER/usapp:latest'
                    sh 'docker push $DOCKER_USER/usapp:latest'
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout || true'
        }
    }
}
