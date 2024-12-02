pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '231791477922'
        AWS_REGION = 'ap-northeast-1'  // e.g., 'us-west-2'
        ECR_REPO_NAME = 'adservice'  // ECR repository name 231791477922.dkr.ecr.ap-northeast-1.amazonaws.com/adservice
        IMAGE_TAG = 'latest'  // or you can use a dynamic tag (e.g., ${GIT_COMMIT})
    }

    stages 
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Login to AWS ECR') {
            steps {
                script {
                    // Login to AWS ECR
                    withAWS(credentials: 'aws-credentials') {
                        sh """
                            aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                        """
                    }
                }
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                script {
                    // Push Docker image to AWS ECR
                    sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }
}
