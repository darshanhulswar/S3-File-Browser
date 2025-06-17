pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY     = credentials('AWS_ACCESS_KEY')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_REGION            = credentials('AWS_REGION')
        BUCKET_NAME           = credentials('BUCKET_NAME')
    }   

    stages {
        stage('git checkout') {
            steps {
                git url: 'https://github.com/darshanhulswar/S3-File-Browser.git', branch: 'master'
            }
        }
        stage('build and tag Dockerfile') {
            steps{
                sh 'docker build -t darshanhulswar/s3-file-browser:latest .'
            }
        }
        stage("containerisation") {
        steps {
            sh '''
                docker run -it -d --name s3-file-browser \
                    -e AWS_ACCESS_KEY=$AWS_ACCESS_KEY \
                    -e AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY \
                    -e AWS_REGION=$AWS_REGION \
                    -e BUCKET_NAME=$BUCKET_NAME \
                    -p 9002:8501 \
                    darshanhulswar/s3-file-browser:latest
            '''
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
                sh 'docker push darshanhulswar/s3-file-browser:latest'
            }
        }
    }
}
