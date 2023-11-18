# How to monitoring Jenkins using the Splunk?

## Step -1: Create a Splunk account

To create a free trail Splunk account using the below link

    http://splunk.com/

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/6feceae3-e4b2-4fdc-82ee-a97277f7b055)

Select -> Products -> Free Trail & Downloads -Splunk enterprise -> Get my free trails

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/204bbfe6-6d51-4458-942d-6debd59c0692)

Choose Linux and select the .deb package -> Download now -> Copy the download link

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/b9a0b2b7-80f9-41ba-9cdb-ac3c2ddc8c2b)

## Step -2: Launch EC2 instance for Jenkins

To launch ubuntu-22 EC2 instance with t3.medium for Jenkins and make ensure the storage sapce > 20 GB

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/9d59408c-36bc-4464-8230-be8245cc9c6d)

### Install Jenkins

    sudo apt update -y
    sudo apt upgrade -y
    wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo apt-key add -
    echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
    sudo apt update
    sudo apt install temurin-17-jdk
    /usr/bin/java --version
    curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
              /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
              https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
                          /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update -y
    sudo apt-get install jenkins -y
    sudo systemctl start jenkins
    sudo systemctl status jenkins

### Access Jenkins console

Access the Jenkins server public ip and port number - 8080. 

Firstly unlock it using the default credentials.

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/90ac3a7b-0515-493f-ba7e-00e04c0748e9)

Install the suggested plugins.

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/76fe6de3-7b3d-495c-98c3-1e5083ea11f3)

Create an admin user to continue.

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/4491d7d6-b626-40b4-a680-a7f33158475b)

## Step -2: Setup a Splunk
