
## 📁 Project Structure

```
devops-eval-yourname/
│
├── pom.xml
├── Dockerfile
├── Jenkinsfile
├── k8s/
│   └── deployment.yaml
│
└── src/
    ├── main/java/com/devops/App.java
    └── test/java/com/devops/AppTest.java
```

---

# 🔹 Phase 1: Clone Project

```bash
git clone https://github.com/Tharun-coder-hash/maven_app.git
cd maven_app
code .
```

---

# 🔹 Phase 2: Java Application

### 📄 `App.java`

```java
package com.devops;

public class App {

    // Odd or Even
    public static String checkOddEven(int num) {
        return (num % 2 == 0) ? "Even" : "Odd";
    }

    // Sum of digits
    public static int sumOfDigits(int num) {
        int sum = 0;
        while (num != 0) {
            sum += num % 10;
            num /= 10;
        }
        return sum;
    }

    // Reverse number
    public static int reverseNumber(int num) {
        int rev = 0;
        while (num != 0) {
            rev = rev * 10 + num % 10;
            num /= 10;
        }
        return rev;
    }

    public static void main(String[] args) {
        System.out.println("Number Utility Service Running...");
    }
}
```

---

# 🔹 Phase 3: JUnit Test

### 📄 `AppTest.java`

```java
package com.devops;

import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class AppTest {

    @Test
    void testOddEven() {
        assertEquals("Even", App.checkOddEven(4));
        assertEquals("Odd", App.checkOddEven(7));
    }

    @Test
    void testSumOfDigits() {
        assertEquals(6, App.sumOfDigits(123));
        assertEquals(15, App.sumOfDigits(555));
    }

    @Test
    void testReverseNumber() {
        assertEquals(321, App.reverseNumber(123));
        assertEquals(1, App.reverseNumber(100));
    }
}
```

---

# 🔹 Phase 4: Maven (`pom.xml`)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.devops</groupId>
    <artifactId>number-service</artifactId>
    <version>1.0</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.10.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>

            <!-- Executable JAR -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>com.devops.App</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>

            <!-- Run tests -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.1.2</version>
            </plugin>

        </plugins>
    </build>
</project>
```

---

# 🔹 Phase 5: Dockerfile (Multi-Stage)

```dockerfile
# Stage 1: Build
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Run
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

# 🔹 Phase 6: Jenkinsfile (Windows FIXED)

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "number-service:latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/Tharun-coder-hash/maven_app.git'
            }
        }

        stage('Build & Test') {
            steps {
                bat 'mvn clean test package'
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t %DOCKER_IMAGE% ."
            }
        }

        stage('Kubernetes Deploy') {
            steps {
                bat "kubectl apply -f k8s/deployment.yaml"
            }
        }
    }
}
```

---

# 🔹 Phase 7: Kubernetes Deployment

### 📄 `k8s/deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: number-service

spec:
  replicas: 2

  selector:
    matchLabels:
      app: number-service

  template:
    metadata:
      labels:
        app: number-service

    spec:
      containers:
      - name: number-app
        image: number-service:latest
        imagePullPolicy: IfNotPresent

        ports:
        - containerPort: 8080
```

---

# 🔹 Phase 8: Commands (Execution Flow)

```bash
# Maven Build
mvn clean test
mvn clean package

# Docker Build
docker build -t number-service:latest .

# Kubernetes (after enabling or minikube start)
kubectl apply -f k8s/deployment.yaml

# Verify
kubectl get pods
```

---

# 🔹 Phase 9: Git Push

```bash
git add .
git commit -m "DevOps complete flow"
git push
```

---

# 🧠 FINAL FLOW (WRITE IN EXAM)

```
GitHub → Maven Build → JUnit Test → Docker Image → Jenkins Pipeline → Kubernetes Deployment (2 Pods)
```

---

# ⚠️ IMPORTANT NOTES

* Maven creates `.jar` file
* Docker packages `.jar` into container
* Jenkins automates build + deploy
* Kubernetes runs **2 replicas**
* Use `bat` in Jenkins for Windows

---


👉 I can compress this into **10-line answer (for 5 marks)**
👉 Or give **diagram for drawing in exam**
