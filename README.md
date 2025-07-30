CI/CD - Continues Integration and Continues Delivery

**Jenkins Installation on Ec2 t2.medium:**

        sudo apt update -y
        sudo apt install openjdk-17-jre -y
        java -version
        vim jenkins.sh
        curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
          /usr/share/keyrings/jenkins-keyring.asc > /dev/null
        echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
          https://pkg.jenkins.io/debian binary/ | sudo tee \
          /etc/apt/sources.list.d/jenkins.list > /dev/null
        sudo apt-get update -y
        sudo apt-get install jenkins -y
        sh jenkins.sh
        sudo cat /var/lib/jenkins/secrets/initialAdminPassword

=========== Docker Installation on same instance ===========

        sudo apt install docker.io -y
        sudo su
        usermod -aG docker jenkins
        usermod -aG docker ubuntu
        systemctl restart docker
        su - jenkins
        docker run hello-world
        docker images

* Jenkins restart from UI: http://65.2.148.254:8080/

Manage Jenkins    -->    Plugins    -->    Available plugins    -->    Search for "Docker Pipeline"

**Tip**: Don't start with a free-style project. Start with **Pipeline** instead as free-style is not declarative approach and not used in production.

* Create Pipeline "first-jenkins"
* Go to pipeline section
* Defintion "Pipeline script from SCM"
* Git Repository URL "https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero.git"
* Credentials "None"
* Branch "/main"
* Script path "my-first-pipeline/Jenkinsfile"
* Apply & Save
* Once, pipeline is completed check on the vm a new docker image called "node-16-alpine" must created.

**Code Snippet of Jenkinsfile**

pipeline {
  agent {
    docker { image 'node:16-alpine' }
  }
  stages {
    stage('Test') {
      steps {
        sh 'node --version'
      }
    }
  }
}

* In above code we can add multiple stages as per our need for Example:

        pipeline {
          agent {
            docker { image 'node:16-alpine' }
          }
          stages {
            stage('Test') {
              steps {
                sh 'node --version'
              }
            }
            stage('Deploy') {
              steps {
                sh kubectl apply -f deployment.yaml
              }
            }
            stage('Test') {
              steps {
                sh 'node --version'
              }
            }
          }
        }


**Multi Stage Multi Agent**

* A "multi-stage multi-agent Jenkins pipeline" refers to a Jenkins Pipeline configuration where the overall automation process is broken down into distinct stages, and each of these stages can be executed on a different Jenkins agent (also known as a slave or node).

**Multi-Stage:**

* Checkout: Fetching source code from a version control system.
* Build: Compiling code and packaging artifacts.
* Test: Running unit tests, integration tests, or end-to-end tests.
* Deploy: Deploying the application to various environments (development, staging, production).
* Post-build actions: Notifications, cleanup, or reporting.

**Multi-Agent:**

Jenkins agents are separate machines or containers connected to the Jenkins master, providing the computational resources to execute pipeline steps. In a multi-agent setup, different stages of the pipeline can be assigned to run on specific agents based on their capabilities, environment, or required tools. For example:

* A build stage might run on an agent with specific compiler versions installed.
* A test stage might run on an agent configured with various operating systems or browsers for cross-platform testing.
* A deployment stage might run on an agent with network access to target deployment environments.



    
    
