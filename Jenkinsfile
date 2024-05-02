pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo/your-python-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'python -m venv env'
                sh 'env/bin/pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                sh 'env/bin/python -m unittest discover tests/'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("your-python-app:${env.BUILD_ID}", '.')
                }
            }
        }

        stage('Deploy to K8s') {
            steps {
                script {
                    kubernetesDeploy(
                        configs: 'path/to/deployment.yaml',
                        kubeconfigId: 'your-k8s-cluster'
                    )
                }
            }
        }
    }

    post {
        always {
            junit 'tests/*.xml'
        }
    }
}
