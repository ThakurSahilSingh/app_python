pipeline {
    agent any

    environment {
        PYTHON_HOME = '/usr/bin/python3'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ThakurSahilSingh/app_python.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh '''
                apt update
                apt install python3 python3-pip -y
                pip3 install -r requirements.txt
                '''
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest tests/ --junitxml=test-results.xml'
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
