pipeline {
    environment {
        backend = 'backend'  // Backend Docker image name
        frontend = 'frontend'  // Frontend Docker image name
        docker_image = ''  // Placeholder
    }

    agent any

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the Git repository...'
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                echo 'Building backend Docker image...'
                dir('backend') {
                    sh "docker build -t your_dockerhub_username/${backend} ."
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                echo 'Building frontend Docker image...'
                dir('frontend') {
                    sh "docker build -t your_dockerhub_username/${frontend} ."
                }
            }
        }

        stage('Push Docker Images to DockerHub') {
            steps {
                echo 'Pushing Docker images to DockerHub...'
                script {
                    docker.withRegistry('', 'DockerHubCred') {
                        sh "docker push your_dockerhub_username/${backend}"
                        sh "docker push your_dockerhub_username/${frontend}"
                    }
                }
            }
        }

        stage('Deploy with Ansible') {
            steps {
                echo 'Running Ansible playbook for deployment...'
                ansiblePlaybook(
                    playbook: 'Deployment/deploy.yml',
                    inventory: 'Deployment/inventory',
                    extras: '--vault-password-file /etc/ansible/vault_password.txt',
                    credentialsId: 'localhost',
                    disableHostKeyChecking: true
                )
            }
        }
    }
}
