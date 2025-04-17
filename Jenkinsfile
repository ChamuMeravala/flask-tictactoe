pipeline {
    agent any

    stages {
        stage ('clone repo') {
           steps {
               git 'https://github.com/ChamuMeravala/flask-tictactoe.git'
           }
        }
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('SonarQube Scan') {
            when {
                expression { return false } // Skip for now
            }
            steps {
                echo "SonarQube scan would go here"
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t flask-tictactoe .'
                }
            }
        }

        stage('Run App') {
            steps {
                sh 'docker run -d -p 5000:5000 flask-tictactoe'
            }
        }
    }
}
         

