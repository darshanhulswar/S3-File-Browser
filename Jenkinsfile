pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                git branch: 'https://github.com/darshanhulswar/S3-File-Browser.git', branch: 'main'
            }
        }
        stage('build and tag Dockerfile') {
            steps{
                sh 'docker build -t darshanhulswar/s3-file-browser:latest .'
            }
        }
        stage("containerisation") {
            steps{
                sh 'docker run -it -d --name c1 -p 9002:8501 darshanhulswar/s3-file-browser:latest'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }
                }
            }              
        }
        stage('Pushing image to Docker Hub repository') {
            steps{
                sh 'docker push darshanhulswar/project:1'
            }
        }
    }
}