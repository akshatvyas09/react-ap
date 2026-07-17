pipeline {

    agent any

    environment {
        IMAGE_NAME = "admin10987/react-app"
        IMAGE_TAG = "latest"
    }


    stages {


        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/akshatvyas09/react-ap.git'
            }
        }


        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(
                        "${IMAGE_NAME}:${IMAGE_TAG}",
                        "./client"
                    )
                }
            }
        }


        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {

                    sh '''
                    echo $DOCKER_PASS | docker login \
                    -u $DOCKER_USER \
                    --password-stdin
                    '''

                }
            }
        }



        stage('Push Docker Image') {
            steps {
                sh '''
                docker push ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }



        stage('Cleanup') {
            steps {
                sh '''
                docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true
                '''
            }
        }

    }


    post {

        success {
            echo "Docker image pushed successfully!"
        }

        failure {
            echo "Pipeline failed!"
        }

    }

}
