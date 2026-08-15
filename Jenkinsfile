pipeline {

    agent any

    environment {
        AWS_REGION   = 'ap-south-1'
        DOCKER_IMAGE = 'anushperamaiyang/trend-app'
        IMAGE_TAG    = "${BUILD_NUMBER}"
        EKS_CLUSTER  = 'trend-cluster'
        K8S_NAMESPACE = 'trend'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Pre-Built dist') {
            steps {
                sh '''
                    echo "Checking pre-built React dist folder..."

                    if [ ! -d "dist" ]; then
                        echo "ERROR: dist folder not found!"
                        exit 1
                    fi

                    echo "dist folder found."
                    ls -lah dist/
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    echo "Building Docker image..."

                    docker build \
                      -t ${DOCKER_IMAGE}:${IMAGE_TAG} \
                      -t ${DOCKER_IMAGE}:latest \
                      .
                '''
            }
        }

        stage('DockerHub Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'aws-docker',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASSWORD" | docker login \
                          -u "$DOCKER_USERNAME" \
                          --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                    echo "Pushing Docker image..."

                    docker push ${DOCKER_IMAGE}:${IMAGE_TAG}
                    docker push ${DOCKER_IMAGE}:latest
                '''
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh '''
                    echo "Updating EKS kubeconfig..."

                    aws eks update-kubeconfig \
                      --region ${AWS_REGION} \
                      --name ${EKS_CLUSTER}

                    echo "Applying Kubernetes manifests..."

                    kubectl apply \
                      -f k8s/namespace.yaml \
                      -f k8s/deployment.yaml \
                      -f k8s/service.yaml \
                      -f k8s/ingress.yaml

                    echo "Updating deployment image..."

                    kubectl -n ${K8S_NAMESPACE} set image \
                      deployment/trend-app \
                      trend-app=${DOCKER_IMAGE}:${IMAGE_TAG}

                    echo "Waiting for rollout..."

                    kubectl -n ${K8S_NAMESPACE} rollout status \
                      deployment/trend-app \
                      --timeout=180s
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    echo "Checking Kubernetes deployment..."

                    kubectl -n ${K8S_NAMESPACE} get deployment trend-app

                    echo "Checking pods..."

                    kubectl -n ${K8S_NAMESPACE} get pods

                    echo "Checking service..."

                    kubectl -n ${K8S_NAMESPACE} get svc
                '''
            }
        }
    }

    post {

        success {
            echo '============================================='
            echo 'Trend CI/CD Pipeline Completed Successfully'
            echo '============================================='
        }

        failure {
            echo '============================================='
            echo 'Trend CI/CD Pipeline Failed'
            echo '============================================='
        }

        always {
            sh 'docker logout || true'
        }
    }
}
