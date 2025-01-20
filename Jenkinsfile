pipeline {
    agent any

    tools {
        maven "maven"
    }

    stages {
        stage('Build') {
            steps {
                git 'https://github.com/umar-fayaaz/muf_group.git'
                sh "mvn clean package -DskipTests"
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def imageTag = "umarfayaaz/muf_group"
                    sh "docker build -t ${imageTag} /var/lib/jenkins/workspace/job"
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Docker login
                    sh '''
                        docker login -u "${DOCKER_USERNAME}" -p "${DOCKER_PASSWORD}"
                    '''

                    // Push the Docker image to Docker Hub
                    def imageTag = "umarfayaaz/muf_group"
                    sh "docker push ${imageTag}"

                    // Log out from Docker Hub (optional)
                    sh "docker logout"
                }
            }
        }
        stage('Clean Up Local Images') {
            steps {
                script {
                    // Delete the local Docker image after pushing it to Docker Hub
                    def imageTag = "umarfayaaz/muf_group"
                    sh "docker rmi ${imageTag} || true"  // The '|| true' ensures the script doesn't fail if the image doesn't exist
                }
            }
        }
        stage('Pull Image from Docker Hub') {
            steps {
                script {
                    // Pull the image from Docker Hub
                    def imageTag = "umarfayaaz/muf_group"
                    sh "docker pull ${imageTag}"
                }
            }
        }
        stage('Run Container') {
            steps {
                script {
                    // Run a container from the pulled image and map port 8081 on the container to port 8081 on the host
                    def imageTag = "umarfayaaz/muf_group"
                    sh "docker run -d -p 8081:8081 --name muf-container ${imageTag}"
                }
            }
        }
    }

    environment {
        DOCKER_USERNAME = "umarfayaaz"  // Replace with your Docker Hub username
        DOCKER_PASSWORD = "#Umar786786"  // Replace with your Docker Hub password
    }
}
