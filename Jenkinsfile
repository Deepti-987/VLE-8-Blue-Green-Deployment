pipeline {
    agent any

    stages {
        stage('Build Green Docker Image') {
            steps {
                sh '''
                sudo -H -u ubuntu /bin/bash -lc \
                "minikube image build -t myapp:v2 '$WORKSPACE/green'"
                '''
            }
        }

        stage('Deploy Green') {
            steps {
                sh '''
                sudo -H -u ubuntu /bin/bash -lc \
                "kubectl apply -f '$WORKSPACE/kubernetes/deployment-green.yaml' &&
                 kubectl rollout status deployment/myapp-green"
                '''
            }
        }

        stage('Test Green') {
            steps {
                sh '''
                sudo -H -u ubuntu /bin/bash -lc \
                "kubectl get pods -l color=green"
                '''
            }
        }

        stage('Switch Traffic') {
            steps {
                input message: 'Green deployment tested. Switch traffic to Green?',
                      ok: 'Switch to Green'

                sh '''
                sudo -H -u ubuntu /bin/bash -lc \
                "kubectl patch service myapp-service \
                -p '{\\"spec\\":{\\"selector\\":{\\"app\\":\\"myapp\\",\\"color\\":\\"green\\"}}}'"
                '''
            }
        }
    }

    post {
        success {
            echo 'Blue-Green deployment completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check Jenkins console output.'
        }
    }
}
