# hello-devops

Go to https://github.com
 and log in to your account.

Click the “+” icon in the top right corner and select “New repository”.

Fill out the repository details:

Repository name: hello-devops

Description: (optional) e.g. "A starter project for DevOps practice."

Public or Private: choose as per your preference.

✅ Check the box “Initialize this repository with a README”

Click Create repository.

You now have a repository named hello-devops with a README.md file inside.

✅ Option 2: Using Command Line (Git CLI)
Prerequisites:

Git must be installed (git --version)

You must be authenticated to GitHub via SSH or a personal access token

🔧 Steps:

Create a local folder:

mkdir hello-devops
cd hello-devops


Initialize Git and add a README.md:

git init
echo "# hello-devops" > README.md
git add README.md
git commit -m "Initial commit with README"


Create a new GitHub repo from the command line:

If you're using the GitHub CLI (gh), run:

gh repo create hello-devops --public --source=. --remote=origin --push


This will create the GitHub repository and push your local code (including the README.md) to GitHub.


Your repository hello-devops will now be live at:

https://github.com/mohamadfarook/hello-devops


It will contain:

hello-devops/
└── README.md

Build a simple Java program that prints ‘Hello DevOps World


Java Code: HelloDevOps.java
public class HelloDevOps {
    public static void main(String[] args) {
        System.out.println("Hello DevOps World");
    }
}

🔧 How to Compile and Run:

Save the code in a file named HelloDevOps.java.

Open your terminal or command prompt.

Navigate to the directory where the file is saved.

Compile the program:

javac HelloDevOps.java


Run the compiled program:

java HelloDevOps

3 Use Maven to compile and package the Java program.

Create a Maven project directory structure like this:

hello-devops/
├── pom.xml
└── src/
    └── main/
        └── java/
            └── com/
                └── example/
                    └── HelloDevOps.java

✅ 2. Java File: HelloDevOps.java

📁 hello-devops/src/main/java/com/example/HelloDevOps.java

package com.example;

public class HelloDevOps {
    public static void main(String[] args) {
        System.out.println("Hello DevOps World");
    }
}

✅ 3. Maven Build File: pom.xml

📁 hello-devops/pom.xml

<project xmlns="http://maven.apache.org/POM/4.0.0" 
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
         
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>hello-devops</artifactId>
    <version>1.0-SNAPSHOT</version>

    <build>
        <plugins>
            <!-- Plugin to allow running the Java main class -->
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>3.1.0</version>
                <configuration>
                    <mainClass>com.example.HelloDevOps</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>

✅ 4. Build the Project

Open a terminal in the hello-devops directory and run:

mvn clean package


This will:

Compile your Java code.

Package it into a .jar file in the target/ directory.

✅ 5. Run the Program

You can run the program using:

mvn exec:java


Or, if you want to run the .jar:

java -cp target/hello-devops-1.0-SNAPSHOT.jar com.example.HelloDevOps

✅ Output:
Hello DevOps World



Step 1: Create Your Flask App

If you don't already have one, here’s a minimal Flask app example:

app.py

from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Flask inside Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)


requirements.txt

Flask==2.3.3

📦 Step 2: Create a Dockerfile

Dockerfile

# Use an official Python runtime as a parent image
FROM python:3.11-slim

# Set the working directory
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Expose the port the app runs on
EXPOSE 5000

# Define environment variable
ENV FLASK_APP=app.py

# Run the app
CMD ["python", "app.py"]

🏗️ Step 3: Build the Docker Image

In the same directory as your Dockerfile, run:

docker build -t flask-docker-app .


5.......
Prerequisites:

Jenkins installed and running.

Docker installed on the Jenkins agent (or master if it’s running the build).

Your project source code is in a Git repository (GitHub, GitLab, Bitbucket, etc.).

Jenkins has Docker permissions (e.g., Jenkins user added to the docker group).

Step 1: Create a Jenkins Pipeline Job

Open Jenkins UI.

Click New Item.

Enter a name for the job, e.g., build-and-dockerize.

Choose Pipeline and click OK.

Step 2: Configure Pipeline Script

In the Pipeline section, choose Pipeline script and use the following example. This example assumes a Maven Java project but you can adapt to your project build tool.

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'your-dockerhub-username/your-image-name'
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from SCM
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Example Maven build command, replace if using npm, gradle, etc.
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image with tag based on build number
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker Hub (you need to set credentials in Jenkins)
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    }

                    // Push Docker image to registry
                    sh "docker push ${DOCKER_IMAGE}:${DOCKER_TAG}"
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace and Docker images to save space
            cleanWs()
            sh "docker rmi ${DOCKER_IMAGE}:${DOCKER_TAG} || true"
        }
    }
}

Step 3: Set Up Credentials in Jenkins

Go to Manage Jenkins > Credentials > System > Global credentials.

Add a new Username with password credential.

Set ID to dockerhub-creds (or change accordingly in pipeline).

Fill in your Docker Hub username and password.

Step 4: Run the Pipeline

Save the job and click Build Now.

Jenkins will:

Checkout your code.

Build the project.

Build the Docker image.

Push the Docker image to Docker Hub (or your Docker registry).


6....

7	Use Terraform to create a local resource (e.g., a file with content ‘Hello Terraform!’).

Step-by-step

Create a file called main.tf.

Add this Terraform configuration to create a local file with the content.

terraform {
  required_providers {
    local = {
      source  = "hashicorp/local"
      version = "~> 2.0"
    }
  }
}

provider "local" {}

resource "local_file" "example" {
  filename = "hello_terraform.txt"
  content  = "Hello Terraform!"
}


Run the following commands in the directory with main.tf:

terraform init
terraform apply


Terraform will prompt to approve; type yes.

After that, you should have a file named hello_terraform.txt in your current directory with the content:

Hello Terraform!

8.....
8	Write a simple Kubernetes deployment file for your app (you can use Minikube

Got it! Here’s a simple Kubernetes deployment YAML file for a basic app, assuming you have an image called your-app-image:latest (replace this with your actual image):

apiVersion: apps/v1
kind: Deployment
metadata:
  name: simple-app-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: simple-app
  template:
    metadata:
      labels:
        app: simple-app
    spec:
      containers:
      - name: simple-app-container
        image: your-app-image:latest
        ports:
        - containerPort: 80

How to apply it on Minikube:

Save this file as deployment.yaml.

Run:

kubectl apply -f deployment.yaml


Check status with:

kubectl get deployments
kubectl get pods

