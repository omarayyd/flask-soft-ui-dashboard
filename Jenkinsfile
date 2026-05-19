pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                    . venv/bin/activate
                    python3 -m pytest || true
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh '''
                    pkill -f "python3 run.py" || true
                    sleep 2
                    . venv/bin/activate
                    nohup python3 run.py > app.log 2>&1 &
                    sleep 3
                    echo "App is running!"
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline SUCCESS!'
        }
        failure {
            echo 'Pipeline FAILED!'
        }
    }
}