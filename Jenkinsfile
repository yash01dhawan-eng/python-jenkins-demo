pipeline {
    agent any

    environment {
        PYTHON = 'C:\\Users\\user\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/yash01dhawan-eng/python-jenkins-demo.git'
            }
        }

        stage('Debug Workspace') {
            steps {
                bat 'dir'
            }
        }

        stage('Setup Python') {
            steps {
                echo "Checking Python..."
                bat '%PYTHON% --version'
                bat '%PYTHON% -m pip --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies..."
                bat '''
                    if exist requirements.txt (
                        %PYTHON% -m pip install -r requirements.txt
                    ) else (
                        echo requirements.txt NOT FOUND
                        exit 1
                    )
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo "Running app..."
                bat '%PYTHON% app.py'
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running tests..."
                bat '%PYTHON% -m pytest test_app.py -v --tb=short'
            }
        }

        stage('Test Coverage') {
            steps {
                echo "Coverage report..."
                bat '%PYTHON% -m pytest test_app.py --cov=app --cov-report=term-missing || exit 0'
            }
        }

        stage('Build Artifacts') {
            steps {
                echo "Creating artifacts..."
                bat '''
                    if not exist dist mkdir dist
                    copy app.py dist\\
                    copy test_app.py dist\\
                    copy requirements.txt dist\\
                '''
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace..."
            cleanWs()
        }
        success {
            echo "Pipeline SUCCESS 🎉"
        }
        failure {
            echo "Pipeline FAILED ❌ check logs"
        }
    }
}
