# How to monitoring and analysis Jenkins builds using the Splunk?

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

Launch one more ubuntu-22 based EC2 instance with t3.medium and storage could be > 20 GB

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/cfb06578-e79a-49d2-a17e-f320650f1c00)

Copy the download link and download the .deb file using wget command.

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/eaf74523-91f1-41a3-be81-86bd09545c07)

### To install and configure splunk analysis

        sudo apt-get update -y
        sudo apt-get upgrade -y
        wget - O splunk-9.1.1-64e843ea36b1-linux-2.6-amd64.deb https://download.splunk.com/products/splunk/releases/9.1.2/linux/splunk-9.1.2-b6b9c8185839-linux-2.6-amd64.deb
        ls -lh
        
![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/7e72c27e-2fec-4151-8dc8-596ec7a1c0ca)

        sudo dpkg -i splunk-9.1.2-b6b9c8185839-linux-2.6-amd64.deb
        sudo /opt/splunk/bin/splunk enable boot-start  # To enable startup at boot times

### To provide user name and password during boot-start.

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/18620135-1224-4195-9fee-90ac0e2c978e)

### To ensure the 22, 8000 is permitted through the ubuntu firewall and enable it using below commands

        sudo ufw allow openSSH
        sudo ufw allow 8000
        sudo ufw enable

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/9a25580a-73a2-4815-a80e-59a7d38c443e)

### To start the Splunk enterprise application

        sudo /opt/splunk/bin/splunk start

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/4e531f0e-2d52-4e91-9556-f49789ba68b8)

### To access the splunk analysis tool

In order to access the splunk console in browser using splunk server IP with port 8000

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/414cebd1-8ef1-4b4e-b1e3-e881a6ff59f9)

Login the console using username and password which was created during boot-start in splunk machine. Now you land in Splunk dashboard.

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/ac5a5d7b-3a8e-4827-8107-b41ef4130ed3)

### To install the splunk app for Jenkins 

To click on Apps -> Find more apps

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/6fb63be3-501e-4e9c-9207-d0d5bdb3ce26)

Browser more apps -> search -> Jenkins -> Install the Jenkins App

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/7bccee5b-a1b6-46c0-9e59-8f1f65b7d8fb)

To provide username and password to install the app - user name as your registered mail and its password

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/2b874505-f061-4a49-a9c5-118608e0482d)

Click on "Go to home"

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/665c0a2c-6fe4-43b2-bb4c-ee092c392610)

Yes! The splunk app for jenkins has been added in Splunk dashboard.

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/de60e532-a9ed-4748-9082-acc6212a3ff7)

Select -> Settings -> Data inputs

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/8a722d8c-9004-4e57-a105-bd1ed9dcf719)

Then select -> HTTP event collector

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/fe696de5-f213-4ba2-a61a-723d11e34269)

Select Global settings

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/dcc825e8-ffe9-4112-b5d2-c03aa17d3bb3)

        All token should be disable
        Disable SSL
        Port number should be 8088
        Then save

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/6b589802-d584-4a33-96bd-c8293a0cf8ec)

### Generate a new Token

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/3b81dede-5014-4bf8-aa1f-9e3140402a6a)

Click on New token -> Provide a name -> Click -> Next

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/17f2b02c-3e45-41ec-b895-b0d612f2e273)

Review and submit

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/4dc25683-0cca-4462-81e8-6d55b1d00ac7)

Token has been created successfully.

Now Select -> Start searching

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/fa6b6b98-a1a3-4e3b-a2a8-e554f617e232)

Now, Go to settings -> Data inputs -> HTTP event collector -> copy the token

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/245fa2b2-0657-4a14-a8db-e5b7cc79c814)

### To install Splunk plugins in Jenkins

Jenkins -> Manager Jenkins -> Manage Plugins -> Available -> Splunk

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/61f19b21-9bf1-4435-9c37-d59e1ef8f3ae)

Install and without restart.

Jenkins -> Manage Jenkins -> System -> Splunk for Jenkins installations

        HTTP input host - Splunk public ip
        HTTP input port - 8088
        HTTP input token - splunk token
        SSL - Disable
        Jenkins master host name - Jenkin public ip

Then apply and save.

### Enable port in ufw

To enable a port 8088 and check the status

        sudo ufw allow 8088
        sudo ufw status

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/bd21656d-9d40-4e62-ad16-b6617844446f)

### To test the splunk connection

To Test the splunk connection in Jenkins 

Jenkins -> Manage jenkins -> System -> Select the Splunk for Jenkins installations -> Then test the connection

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/6a00ee34-749f-4326-97a1-30b3a4ba9ff5)

Perfect! The splunk connection has been verified.

### To restart the Splunk machine

Click on settings -> server controls

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/38ac34b8-d8e0-4e55-9f8f-a247f55baef3)

The Restart the Splunk

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/74f060bf-eb34-45e2-b4f2-1e1e2ecd472f)

### To restart the Jenkins

        Jenkins ip:8080/restart

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/eb588920-88f5-4361-9c19-5a3e4f9440a3)

## Step -3: To create a Jenkins pipeline

To create a new project with pipeline project

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/e364d4af-d991-441a-907e-1fe918b900bc)

### To add a simple stages for jenkins pipeline

To add a below script on pipeline

        pipeline {
            agent any
        
            stages {
                stage('Build') {
                    steps {
                        sh 'echo "Building the project"'
                    }
                }
        
                stage('Test') {
                    steps {
                        sh 'echo "Running some usecase tests"'
                    }
                }
        
                stage('Deploy') {
                    steps {
                        sh 'echo "Deploying the application on hosts"'
                    }
                }
            }
        }

Then Apply and save - To start the build.

Yes! The build has been succeeded.

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/7f325f87-527a-4cad-b489-7ef5d0fac039)

## Step -4: To check on Splunk dashboard

To login the Splunk dashboard and select "Splunk App for Jenkins"

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/0066ad4a-892b-4fb6-a368-7f725d90007b)

Now to change user as Admin and see the build logs

![image](https://github.com/kohlidevops/Jenkins-Splunk/assets/100069489/50046afe-382c-401a-a95a-cca576f4ae8e)

You can see number of build logs, build duration and build status.

That's it!
