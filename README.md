Jenkins CI/CD Pipeline for Flask Application
Implementation of CI/CD Pipeline for a Python Flask Application using Jenkins

Objective
The objective of this assignment is to design and implement a CI/CD pipeline using Jenkins that automates:

Building a Flask application
Running automated unit tests
Deploying the application to a staging environment
The pipeline must trigger automatically on code changes and provide build status notifications.

Prerequisites
Before starting this assignment, the following are required:

 1 - Linux-based system / EC2 instance
 2 - Python 3 installed
 3 - Jenkins installed and running
 4 - GitHub account
 5 - Basic knowledge of Git, Python, and Jenkins
 
Tools & Technologies Used
 1 - Python 3
 2 - Flask
 3 - Pytest
 4 - Jenkins (Declarative Pipeline)
 5 - Git & GitHub
 6 - GitHub Webhooks
 7 - Linux / EC2

Step 1: Create Flask Application
  
  from flask import Flask

      app = Flask(__name__)

       @app.route('/')
      def home():
       return "Flask Application Successfully Deployed"

      if __name__ == '__main__':
       app.run(host='0.0.0.0', port=5001)


Step 2: Define Application Dependencies

 requirements.txt

   Flask==2.3.2
   Werkzeug==2.3.6
   pytest==7.4.4

Step 3: Create Unit Tests

  tests/test_app.py
    import sys
    import os
      sys.path.append(os.path.dirname(os.path.dirname(__file__)))

        from app import app

        def test_home_page():
         client = app.test_client()
         response = client.get('/')
         assert response.status_code == 200

Step 4: Create Jenkins Pipeline
   
   Jenkinsfile
    A declarative Jenkins pipeline was created with Build, Test, and Deploy stages.

    pipeline {
        agent any

        environment {
           VENV = "venv"
        }

        stages {
           stage('Checkout') {
             steps {
                git branch: 'main',
                    url: 'https://github.com/deepanshusaini1402/jenkinsassignmentherovired.git'
                }
            }

        stage('Build') {
            steps {
                sh '''
                python3 -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                . $VENV/bin/activate
                pytest -v
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                pkill -f "python app.py" || true
                . $VENV/bin/activate
                nohup python app.py &
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}

Step 5: Configure Jenkins Job

Created a Pipeline job in Jenkins
Selected Pipeline script
Manually pasted the script.

<img width="1589" height="761" alt="Screenshot 2026-01-18 182934" src="https://github.com/user-attachments/assets/a64f63e6-04f0-40eb-8a16-a6f4755ce26d" />
<img width="1585" height="803" alt="Screenshot 2026-01-18 182959" src="https://github.com/user-attachments/assets/eee588be-357b-4162-abd9-8e9b02287fe8" />


Step 6: Configure GitHub Webhook

Payload URL: https://delicately-nonspillable-regenia.ngrok-free.dev/github-webhook/

<img width="1599" height="764" alt="image" src="https://github.com/user-attachments/assets/ddf7dd79-46a1-4d1f-9bfb-0a70c20ce5ec" />
<img width="1599" height="765" alt="image" src="https://github.com/user-attachments/assets/7ab7c20c-2269-4943-81e4-f9714b759f74" />

Step 7: Pipeline Execution Flow










