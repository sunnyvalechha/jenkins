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

**Practical:**

We have made changes in the existing pipeline and created a multi-build and multi-state pipeline.

* Modify the folder name at Script path "multi-stage-multi-agent/Jenkinsfile"
* Run the pipeline.
* New docker image have been created "maven-3.8.1-adoptopenjdk-11"


# CI/CD Pipeline

<img width="1743" height="720" alt="image" src="https://github.com/user-attachments/assets/efbe2aef-54e5-4192-bf3a-410c4241c41d" />

**Explanation:**

1. Inside Jenkins we have created jenkins file it can be imported from the source code management tool (Git) or can be written inside jenkins.
2. Purpose of the jenkins file is to execute all the other tasks of the **Continues Integration** only, not for the Continues Delivery.
3. The docker image we are using already has a maven installation.
4. 

**Steps:**

1. Code checkout from Github
2. Configure Sonar server on ec2 instance
3. Setup Sonar & jenkins Integration
4. 


* t2.large ubuntu instance launched
* Setup Jenkins
* Create pipeline project "ultimate-cicd"
* Defintion "Pipeline script from SCM"
* SCM "Git"
* Repo URL "https://github.com/sunnyvalechha/Jenkins-Zero-To-Hero.git"
* No password required as repo is public
* Branch "*/main"
* Script Path "java-maven-sonar-argocd-helm-k8s/spring-boot-app/JenkinsFile"
 
**Tip**: Here, script path can be present on any location and the file name can be anything like JenkinsFile or jenkinsnofile or JENKins

* Manage Jenkins    -->    Plugins    -->    Available plugins    -->    Search for "Docker Pipeline"
* Manage Jenkins    -->    Plugins    -->    Available plugins    -->    Search for "SonarQube Scanner"
* sudo apt update && sudo apt install unzip -y
* adduser sonarqube
* mkdir /opt/sonarqube
* cd /opt/sonarqube
* wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.6.0.92116.zip
* All versions of sonar: https://binaries.sonarsource.com/?prefix=Distribution/sonarqube/
* unzip *
* Edit <sonarqubeHome>/conf/sonar.properties to configure the following settings:
        sonar.path.data=/var/sonarqube/data
        sonar.path.temp=/var/sonarqube/temp
* sudo chmod -R 775 /opt/sonarqube
* sudo chown -R sonarqube:sonarqube /var/sonarqube/
* sudo chown -R sonarqube:sonarqube /opt/sonarqube-10.6.0.92116/
* cd /opt/sonarqube/sonarqube-10.4.1.88267/bin/linux-x86-64
* /opt/sonarqube-10.6.0.92116/bin/linux-x86-64/sonar.sh start
* /opt/sonarqube-10.6.0.92116/bin/linux-x86-64/sonar.sh status
* http://13.235.132.98:9000
* Sonar id:admin / pass:123
* Sonar        -->        Myaccount        -->        Security        -->        Generate Token        --> Copy Token
* Manage Jenkins        -->        Credentials        -->        System        -->        Global credentials (unrestricted)        -->        Add creds        -->
* New creds        -->        Kind-Secret text        -->        Paste token in secret section        -->        Id=sonarqube        -->        Create        
