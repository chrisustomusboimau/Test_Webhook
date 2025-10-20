pipeline {
    agent {
        dockerContainer {
            image 'python:3.10-slim'
        }
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'git-credentials',
                    url: 'https://github.com/chrisustomusboimau/Test_Webhook.git',
                    branch: 'main'
            }
        }

        stage('Setup Environment') {
            steps {
                sh '''
                echo "🔍 Python version:"
                python3 --version

                echo "🧩 Installing dependencies from requirements.txt..."
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                pytest tests/ --junitxml=report.xml || echo "⚠️ No tests found"
                '''
            }
            post {
                always {
                    junit 'report.xml'
                }
            }
        }
    }

    triggers {
        githubPush()
    }
}
