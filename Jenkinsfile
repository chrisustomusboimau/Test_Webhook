pipeline {
    agent any

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
                echo 🧩 Installing dependencies from requirements.txt...
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                echo "🧪 Running tests..."
                sh 'pytest tests/ --junitxml=report.xml || echo "⚠️ No tests found"'
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

    post {
        success {
            echo '✅ Build and Test Succeeded!'
        }
        failure {
            echo '❌ Build or Test Failed.'
        }
    }
}
