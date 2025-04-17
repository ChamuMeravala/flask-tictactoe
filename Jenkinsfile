pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/ChamuMeravala/flask-tictactoe.git'
            }
        }

        stage('Install Dependencies') {
           steps { 
                sh ''' 
                        python3 -m venv venv 
                        .  venv/bin/activate 
                        pip install --upgrade pip 
                        pip install -r requirements.txt 
                '''
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
                sh 'docker build -t flask-tictactoe .'
            }
      
        }
        stage('Run App') {
           steps {
              sh '''
                  docker stop flask-tictactoe || true
                  docker rm flask-tictactoe || true
                  docker run -d -p 5000:5000 --name flask-tictactoe flask-tictactoe
              '''
           }
        } 
     
    }
}            
