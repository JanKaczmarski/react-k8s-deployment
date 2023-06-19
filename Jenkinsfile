pipeline {
    
    environment {
        dockerimagename = "bigjack213/react-app"
        dockerImage = ""
    }

    agent any

    stages {
        stage('Checkout Source') {
            steps{
                git 'https://github.com/JanKaczmarski/jenkins-k8s-deployment'
            }
        }

        stage('Build image') {
            steps{
                script{
                    dockerImage = docker.build(dockerimagename)
                }
            }
        }

        stage('Push image') {
            environment {
                registryCredential = 'dockerhub-credentials'
            }
            steps {
                script {
                    docker.withRegistry(' https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Deploying react app to kubernetes') {
            steps {
                script {
                    kubernetesDeploy(configs: "app-deployment.yaml", "app-load-balancer.yaml")
                }
            }
        }
    }

}