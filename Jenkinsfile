pipeline {
    environment {
        backend = 'backend'  // Backend Docker image name
        frontend = 'frontend'  // Frontend Docker image name
    }

    agent any

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the Git repository...'
                git branch: 'main', url: 'https://github.com/Aasmaan007/paytm-spe-.git'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                echo 'Building backend Docker image...'
                dir('backend') {
                    script {
                        docker.build("aasmaan1/${backend}")
                    }
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                echo 'Building frontend Docker image...'
                dir('frontend') {
                    script {
                        docker.build("aasmaan1/${frontend}")
                    }
                }
            }
        }

       stage('Push Docker Images to DockerHub') {
            steps {
                echo 'Pushing Docker images to DockerHub...'
                script {
                    // Docker login using credentials
                    withDockerRegistry([ credentialsId: 'DockerHubCred', url: 'https://index.docker.io/v1/' ]) {
                        // Push the backend image to DockerHub
                        docker.image("aasmaan1/${backend}").push()
                        // Push the frontend image to DockerHub
                        docker.image("aasmaan1/${frontend}").push()
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
