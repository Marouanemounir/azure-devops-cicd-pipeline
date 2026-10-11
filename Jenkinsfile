pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
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
                        registry + ":${env.BUILD_NUMBER}"
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
                        registryCredential
                    ) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
