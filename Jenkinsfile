pipeline {
    agent any

    stages {
        stage('Checkout stage') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Mshamcodes/Python-AutomationPipeline.git']])
            }
        }
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/Mshamcodes/Python-AutomationPipeline.git'
                sh 'python3 src/arithmetic_operations.py'
            }
        }
        stage('RUN tests') {
            steps {
                sh '''
                    mkdir -p test-results
                    python3 -m pytest -v src/test_arithmetic_operations.py --junitxml=test-results/report.xml
                '''
            }
        }
    }
}
