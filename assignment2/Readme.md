# Jenkins Pipeline for Docker Build and Push

## **Task 2: Create and Build Now**

### **1. Create Task 2 in Jenkins and Click Build Now**
![Create Task 2](https://github.com/user-attachments/assets/e438ecae-ec5a-476b-a6ca-9eaa24b25fee)

### **2. Console Output After Running Build**
![Console Output](https://github.com/user-attachments/assets/35f3e6df-67f8-4540-90d0-a265efb5423e)

### **3. Docker Repository After Push**
![Docker Repository](https://github.com/user-attachments/assets/6fb6a51b-74d2-4ead-b741-d376dcdac8e3)

### **4. Commands Used**
![Commands](https://github.com/user-attachments/assets/57037e2c-cc21-4803-9916-65b63cc7c593)

---

## **Jenkins Pipeline Script**

```groovy
pipeline {
    agent any

    environment {
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
                sh 'docker build -t $APP_NAME:$BUILD_NUMBER .'
            }
        }
        
        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-token', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }
        
        stage('Push Image') {
            steps {
                sh 'docker push $APP_NAME:$BUILD_NUMBER'
            }
        }
    }
}
```

---

## **Instructions to Set Up Jenkins and Run the Pipeline**

### **1. Add Docker Credentials in Jenkins**
- Go to **Jenkins Dashboard → Manage Jenkins → Manage Credentials**.
- Add a new credential with:
  - **ID:** `docker-token`
  - **Username:** Your DockerHub username
  - **Password:** Your DockerHub password

### **2. Run the Pipeline**
1. Open Jenkins, navigate to **Task 2**.
2. Click **Build Now**.
3. Check the **Console Output** to verify the steps.
4. Once complete, check the **DockerHub repository** to confirm the image was pushed.

### **3. Verify Docker Image on Local Machine**
```sh
docker pull ikjas/ikjas:<BUILD_NUMBER>
docker images
docker run -d -p 8080:8080 ikjas/ikjas:<BUILD_NUMBER>
```

### **4. Troubleshooting**
- If DockerHub login fails, check if credentials are correct in **Jenkins Credentials Store**.
- If Docker build fails, ensure Docker is running and you have correct **Dockerfile**.
- If push fails, check network connectivity and repository permissions.

---

### **Outcome:**
After executing this pipeline, your **Docker image** will be built, tagged, and pushed to **DockerHub**, making it accessible for deployment anywhere! 🚀

