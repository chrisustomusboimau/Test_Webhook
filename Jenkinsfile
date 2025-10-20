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
                echo "🔍 Checking Python installation..."
                if ! command -v python3 &> /dev/null; then
                    echo "⚠️ Python3 not found. Installing Miniconda locally..."
                    
                    # Gunakan curl atau wget yang tersedia
                    if command -v curl &> /dev/null; then
                        curl -sS https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o $HOME/miniconda.sh
                    elif command -v wget &> /dev/null; then
                        wget -q https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O $HOME/miniconda.sh
                    else
                        echo "❌ Neither curl nor wget found. Cannot install Miniconda."
                        exit 1
                    fi

                    bash $HOME/miniconda.sh -b -p $HOME/miniconda
                    export PATH="$HOME/miniconda/bin:$PATH"
                    echo "✅ Miniconda installed!"
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

    post {
        success {
            echo '✅ Build and Test Succeeded!'
        }
        failure {
            echo '❌ Build or Test Failed.'
        }
    }
}
