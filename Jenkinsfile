pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'lalithakashyap30/microproject:latest'
        KUBE_NAMESPACE = 'jenkins'
        KUBE_DEPLOYMENT = 'microproject-deployment'
    }
    
    stages {
        stage('Build') {
            steps {
                script {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'npm test'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.DOCKER_IMAGE}")
                }
            }
        }
        stage('Push Docker Image') {
            // This stage is not needed since we won't push the image to a Docker registry
            steps {
                script {
                    echo "Skipping image push stage"
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f kubernetes/deployment.yaml --namespace ${env.KUBE_NAMESPACE}"
                }
            }
        }
        stage('Verify Deployment') {
            steps {
                script {
                    sh "kubectl rollout status deployment/${env.KUBE_DEPLOYMENT} --namespace ${env.KUBE_NAMESPACE}"
                }
            }
        }
    }
}

}
