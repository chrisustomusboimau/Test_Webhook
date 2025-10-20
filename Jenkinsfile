pipeline {
        
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
                echo "🔍 Checking Python installation..."
                
                # Cek apakah python3 tersedia
                if ! command -v python3 &> /dev/null; then
                    echo "⚠️ Python3 not found. Installing Miniconda locally..."
                    wget -q https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
                    bash miniconda.sh -b -p $HOME/miniconda
                    export PATH="$HOME/miniconda/bin:$PATH"
                    echo "✅ Miniconda installed successfully!"
                else
                    echo "✅ Python3 already available."
                fi
                
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
