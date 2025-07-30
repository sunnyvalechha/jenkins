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

Manage Jenkins    -->    Plugins    -->    Available plugins    -->    Docker Pipeline    -->    

**Tip**: Don't start with a free-style project. Start with **Pipeline** instead as free-style is not declarative approach and not used in production.

* Create Pipeline "first-jenkins"
* Go to pipeline section
* Defintion "Pipeline script from SCM"
* Git Repository URL "https://github.com/iam-veeramalla/Jenkins-Zero-To-Hero.git"
* Credentials "None"
* Branch "/main"
* Script path "my-first-pipeline/Jenkinsfile"
* Apply & Save







    
    
