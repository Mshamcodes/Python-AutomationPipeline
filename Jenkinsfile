pipeline {
    agent {
        docker {
            image 'python:3.13.2'  // Docker agent with Python & pip pre-installed
        }
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Pulls from GitHub
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Pytest') {
            steps {
                // Ensure test-results folder exists
                sh 'mkdir -p test-results'
                // Run Pytest and export JUnit XML report
                sh 'pytest src/test_automation.py --junitxml=test-results/report.xml'
            }
        }

        stage('Publish Test Report') {
            steps {
                // Let Jenkins read the test report
                junit 'test-results/report.xml'
            }
        }
    }
}
