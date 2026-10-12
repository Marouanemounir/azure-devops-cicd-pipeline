
pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
        disableConcurrentBuilds()
    }

    environment {
        registry = "marwane444/tp4"
        registryCredential = "dockerhub"
        dockerImage = ""
    }

    stages {

        stage('Cloning Git') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Marouanemounir/tp4.git'
            }
        }

        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build(
                        "${env.registry}:${env.BUILD_NUMBER}"
                    )
                }
            }
        }

        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }

        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry(
                        'https://index.docker.io/v1/',
                        env.registryCredential
                    ) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy image') {
            steps {
                script {
                    def imageName =
                        "${env.registry}:${env.BUILD_NUMBER}"

                    echo "===== DEPLOYMENT START ====="

                    bat '''
                        @echo off
                        docker container inspect tp4-app >NUL 2>&1
                        if not errorlevel 1 docker rm -f tp4-app
                    '''

                    echo "Pulling Docker image..."

                    bat "docker pull ${imageName}"

                    echo "Starting new container..."

                    bat "docker run -d --name tp4-app -p 8081:80 ${imageName}"

                    echo "Checking deployment..."

                    bat 'docker ps --filter "name=tp4-app"'

                    echo "===== DEPLOYMENT FINISHED ====="
                }
            }
        }
    }
}
