![Screenshot (335)](https://github.com/user-attachments/assets/e438ecae-ec5a-476b-a6ca-9eaa24b25fee)

create task 2 and click build now
![Screenshot (332)](https://github.com/user-attachments/assets/9f2042b2-e1c9-4251-989c-4b2f29a859b4)

console output
![Screenshot (333)](https://github.com/user-attachments/assets/35f3e6df-67f8-4540-90d0-a265efb5423e)

docker repository
![Screenshot (330)](https://github.com/user-attachments/assets/6fb6a51b-74d2-4ead-b741-d376dcdac8e3)

commends
![Screenshot (334)](https://github.com/user-attachments/assets/57037e2c-cc21-4803-9916-65b63cc7c593)

**script file**


pipeline { 
    agent any 

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-token')
        APP_NAME = "ikjas/ikjas" 
    }

    stages { 
        stage('SCM Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/lily4499/lil-node-app.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {  
                sh 'docker build -t $APP_NAME:$BUILD_NUMBER .' // Fixed missing '-' in '-t'
            }
        }
        
        stage('Login to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        
        stage('Push Image') {
            steps {
                sh 'docker push $APP_NAME:$BUILD_NUMBER'
            }
        }
    }
}
