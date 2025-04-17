# Tic Tac Toe Flask App with Jenkins CI/CD and Docker Deployment

This project demonstrates how to set up a basic Flask app (Tic Tac Toe placeholder) with Jenkins CI/CD integration and Docker-based deployment on an AWS EC2 instance.

---

## 🔧 Prerequisites
- AWS EC2 instance (Ubuntu)
- Jenkins installed and running
- Docker installed and configured
- GitHub repository with Flask app
- Flask installed in your project

---

## 📁 Project Structure
```
flask-tictactoe/
├── app.py
├── requirements.txt
├── Dockerfile
└── Jenkinsfile
```

---

## 1️⃣ Flask App Setup
**app.py**
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Tic Tac Toe!"

if __name__ == '__main__':
    app.run(debug=False, host='0.0.0.0')
```

**requirements.txt**
```
flask
```

---

## 2️⃣ Docker Setup
**Dockerfile**
```Dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

**Build and Run Docker locally**
```bash
docker build -t flask-tictactoe .
docker run -d -p 5000:5000 --name flask-tictactoe flask-tictactoe
```

---

## 3️⃣ Jenkins Setup

### ✅ Install Jenkins Plugins
- Git Plugin
- Pipeline Plugin
- Docker Plugin (optional)

### ✅ Create New Pipeline Job
1. Open Jenkins Dashboard → New Item → Pipeline
2. Name: `Flask-TicTacToe-Pipeline`
3. Choose: *Pipeline*
4. Pipeline script from SCM → Git
5. Repository URL: `https://github.com/yourusername/flask-tictactoe.git`
6. Branch Specifier: `*/main`

---

## 4️⃣ Jenkinsfile
**Jenkinsfile**
```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yourusername/flask-tictactoe.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-tictactoe .'
            }
        }

        stage('Deploy') {
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
```

---

## 🛠️ Jenkins Pipeline Run Output
- Successfully cloned repo
- Built Docker image
- Deployed container

You can verify the container is running:
```bash
docker ps
```

Check logs:
```bash
docker logs flask-tictactoe
```

---

## 🌐 Access the App
Ensure port 5000 is **open** in your AWS EC2 security group.

Now access the app:
```
http://<your-ec2-public-ip>:5000
```
Example:
```
http://3.87.193.215:5000
```
You should see:
```
Hello from Tic Tac Toe!
```

---

## 📌 Notes
- If port 5000 is already used, free it:
```bash
lsof -i :5000
kill <PID>
```
- If you get permission denied on Docker:
```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

---

To Do (Optional Next Steps):
- Add UI for the Tic Tac Toe game
- Use Gunicorn + Nginx for production deployment
- Add automated tests to Jenkins pipeline
- Use GitHub Webhook to trigger builds
- Secure Jenkins instance and pipeline

