pipeline {
    agent any

    environment {
        GIT_REPOSITORY_URL = 'https://github.com/Prathamesh-44/docker_jenkins_demo.git'
        DOCKER_IMAGE_NAME = 'pd44/docker_jenkins_demo'
        IMAGE_TAG = '1.0'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    try {
                        git branch: 'main', url: GIT_REPOSITORY_URL
                    } catch (Exception e) {
                        echo "Failed to clone repository: ${e.message}"
                        error "Failed to clone repository"
                    }
                }
            }
        }

       // stage('Build Docker Image') {
         //   steps {
          //      script {
           //         try {
             //           docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
               //     } catch (Exception e) {
                 //       echo "Failed to build Docker image: ${e.message}"
                   //     error "Failed to build Docker image"
                    //}
               // }
           // }
        //}
stage('Build Docker Image') {
    steps {
        script {
            // Option A: Explicitly log out to ensure public images pull anonymously without 401 errors
            sh "docker logout" 
            sh "docker build -t pd44/docker_jenkins_demo:1.0 ."
            
            // OR Option B: Wrap it inside your valid credentials block if you want to pull as a logged-in user
            // withCredentials([usernamePassword(credentialsId: 'your-docker-hub-credentials-id', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
            //     sh "echo \$PASS | docker login -u \$USER --password-stdin"
            //     sh "docker build -t pd44/docker_jenkins_demo:1.0 ."
            // }
        }
    }
}

        stage('Push to DockerHub') {
            steps {
                script {
                    try {
                        withCredentials([usernamePassword(credentialsId: 'my-docker-hub-credentials-id', 
                                                         usernameVariable: 'DOCKER_USERNAME', 
                                                         passwordVariable: 'DOCKER_PASSWORD')]) {
                            // Explicit login before push
                            sh """
                            echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                            docker push ${DOCKER_IMAGE_NAME}:${IMAGE_TAG}
                            """
                        }
                    } catch (Exception e) {
                        echo "Failed to push Docker image to registry: ${e.message}"
                        error "Failed to push Docker image"
                    }
                }
            }
        }
    }
}
