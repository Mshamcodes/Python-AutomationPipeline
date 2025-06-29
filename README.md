# Python-AutomationPipeline
Python Automation running on Jenkins Pipeline

Jenkins + Pytest CI Pipeline for Python Automation

This README.md is a complete step-by-step guide to setting up Pytest-based automation using Jenkins CI integrated with GitHub. This project demonstrates how to:

Run Python automation with Pytest

Automate test execution through Jenkins pipeline

View test reports in Jenkins UI

📌 Objective

Build a Python automation project that runs test cases on Jenkins every time a change is pushed to GitHub.

🛠️ Tools & Technologies

Jenkins (running inside Docker)

GitHub (source control)

Pytest (test framework)

JUnit Report Plugin (for report publishing)

📂 Repository Structure

Python-AutomationPipeline/
├── src/
│   └── arithmetic_operations.py               # Python source code file
    └── test_arithmetic_operations.py          # Python test file
├── requirements.txt                           # Python dependencies
├── Jenkinsfile                                # Jenkins pipeline definition
└── README.md                                  # Summary file


🔗 Linking GitHub Repo with Jenkins Pipeline

Step-by-step:

Open Jenkins → New Item → Enter Name: Python-AutomationPipeline → Select Pipeline → Click OK.

Go to Configure → Scroll to Pipeline section.

Select Pipeline script from SCM.

Choose Git and enter your repository URL:

https://github.com/Mshamcodes/Python-AutomationPipeline.git

Set Branch Specifier to */main.

Save the job.

Trigger builds manually or set up GitHub webhooks for auto-triggering (requires public Jenkins server).



✅ Issues We Faced & Fixes

❌ pytest: not found

Reason: Jenkins container didn’t have pytest installed.

Fix: Manually installed pytest using:

docker exec -u root -it <jenkins-container-id> bash
apt-get update
apt-get install -y python3-pip
pip install pytest

Alternatively:

pip install pytest --break-system-packages

❌ pip: not found or Permission denied

Reason: Jenkins pipeline cannot install packages due to lack of root access.

Fix: Avoid installing system packages during the pipeline. Pre-install them inside the Jenkins container manually or by extending the Docker image.

❌ Test results not showing in Jenkins Stage View

Fix:

Added the Pipeline Stage View plugin in Jenkins.

Used --junitxml option with Pytest.

Published using junit in Jenkinsfile.

❌ pytest src/test_automation.py fails due to file not found

Fix: Ensured correct file path is referenced inside Jenkins workspace.

✅ Jenkinsfile

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[url: 'https://github.com/Mshamcodes/Python-AutomationPipeline.git']]
                )
            }
        }

        stage('Run Pytest') {
            steps {
                sh 'mkdir -p test-results'
                sh 'pytest -v src/test_automation.py --junitxml=test-results/report.xml'
            }
        }

        stage('Publish Test Report') {
            steps {
                junit 'test-results/report.xml'
            }
        }
    }
}

✅ Sample Pytest Test File: test_automation.py

import math

def test_addition():
    assert 2 + 2 == 4

def test_multiplication():
    assert 3 * 3 == 9

def test_division():
    assert 10 / 2 == 5

def test_sqrt():
    assert math.isclose(math.sqrt(16), 4.0)

✅ requirements.txt

pytest

(Install this manually inside the Jenkins Docker container.)

✅ Jenkins UI Tips

Install Pipeline Stage View Plugin

Use Declarative Pipeline syntax

Go to Pipeline Job → Stage View tab to see progress

✅ Summary

You now have a complete working Jenkins + Pytest pipeline:

✅ Code hosted on GitHub

✅ Jenkins auto-fetches latest code

✅ Runs test cases via Pytest

✅ Publishes results with JUnit plugin

This forms a reusable template for Python automation projects with CI/CD capabilities.