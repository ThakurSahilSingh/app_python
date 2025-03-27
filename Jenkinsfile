pipeline {
    agent any

    environment {
        PYTHON_HOME = '/usr/bin/python3'
        VENV_PATH = 'venv'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ThakurSahilSingh/app_python.git'
            }
        }
        stage('Setup Virtual Environment') {
            steps {
                sh '''
                ${PYTHON_HOME} -m venv ${VENV_PATH}
                source ${VENV_PATH}/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh '''
                source ${VENV_PATH}/bin/activate
                pytest tests/ --junitxml=test-results.xml
                '''
            }
        }
        stage('Build Application') {
            steps {
                sh '''
                mkdir -p build
                tar -czvf build/app.tar.gz src/*.py
                '''
            }
        }
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'build/*.tar.gz', fingerprint: true
            }
        }
    }
}
