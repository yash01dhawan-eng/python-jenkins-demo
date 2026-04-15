pipeline {
    agent any

    environment {
        PYTHON = 'C:\\Users\\user\\AppData\\Local\\Programs\\Python\\Python312\\python.exe'
    }

    stages {

        stage('Setup Python') {
            steps {
                echo "Setting up Python environment..."
                bat '%PYTHON% --version'
                bat '%PYTHON% -m pip --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing Python dependencies..."
                bat '%PYTHON% -m pip install -r requirements.txt'
            }
        }

        stage('Run Application') {
            steps {
                echo "Running the application..."
                bat '%PYTHON% app.py'
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running unit tests..."
                bat '%PYTHON% -m pytest test_app.py -v --tb=short'
            }
        }

        stage('Test Coverage') {
            steps {
                echo "Generating test coverage report..."
                bat '%PYTHON% -m pytest test_app.py --cov=app --cov-report=term-missing || exit 0'
            }
        }

        stage('Build Artifacts') {
            steps {
                echo "Creating build artifacts..."
                bat '''
                    if not exist dist mkdir dist
                    copy app.py dist\\
                    copy test_app.py dist\\
                    copy requirements.txt dist\\
                    echo Artifacts ready in dist directory
                '''
            }
        }
    }

    post {
        always {
            echo "Cleaning up workspace..."
            cleanWs()
        }
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed! Check logs above."
        }
    }
}
