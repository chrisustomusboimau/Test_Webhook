pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'GIT_WEBHOOK',
                    url: 'https://github.com/chrisustomusboimau/Test_Webhook.git',
                    branch: 'main'
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Running build process...'
                sh 'echo "Build step berjalan di Linux agent"'
            }
        }

        stage('Run Tests') {
            steps {
                echo '🧪 Running tests...'
                // Ganti sesuai kebutuhan, misalnya:
                sh 'pytest tests/ --junitxml=report.xml || echo "No tests found"'
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
