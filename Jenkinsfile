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
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        docker.image(DOCKER_IMAGE).push('latest')
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f kubernetes/deployment.yaml --namespace ${KUBE_NAMESPACE}"
                }
            }
        }
        stage('Verify Deployment') {
            steps {
                script {
                    sh "kubectl rollout status deployment/${KUBE_DEPLOYMENT} --namespace ${KUBE_NAMESPACE}"
                }
            }
        }
    }
}
