![npm package](https://img.shields.io/badge/ansible-2.9.20-black.svg)
![npm package](https://img.shields.io/badge/python-2.7.18-turquoise.svg)
![npm package](https://img.shields.io/badge/git-2.23.4-red.svg)
![npm package](https://img.shields.io/badge/jenkins-2.289.1-purple.svg)
![npm package](https://img.shields.io/badge/amazon-aws-yellow.svg)

<h2>Introducing the project</h2>

Here is another nice combination of my favourite tools, AWS, Ansible and Jenkins. In this case, once we launched an EC2 instance from AWS (that is going to be the Ansible controller as well as the Jenkins server) and make some settings on it, a fully CI/CD pipeline will be created demonstrated the Open source music-streamer provided by Ampache.

<Preparations>
  
I have chosen *Amazon Linux2* for Ansible-Jenkins server. In order to Jenkins be installed, we need to do some tasks:
  
  ```bash
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

```bash
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```

```bash
sudo yum upgrade
```
```bash
sudo yum install jenkins chkconfig java-devel
```
```bash
sudo systemctl daemon-reload
```
```bash
sudo systemctl start --now jenkins
```
  
Now the Jenkins is up and running. Let's continue with ansible installation.

<h2>Install Ansible</h2>
There is a repository provided by Amazon Linux, where we can install:
  
  ```bash
  sudo amazon-linux-extras install ansible2
  ```
Once we done that, let's check the current version of ansible:
  ```bash
  ansible --version
  ```
It also returns with the version of python as well as some other informations.
  
Finally, install git as well:
  ```bash
  sudo yum install git
  ```
Now it is time to care for post-installation of Jenkins...
  
<h2>Jenkins Post-installation</h2>
  
Once we logged in with typing \<Public-IP-Address:8080> grab the initial admin password from here:
  ```bash
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  ```
  
Paste it to the appropriate place and customize the Jenkins.
  
<h2>Required plugins</h2>
  
We also need to download some additional plugins:
  
  - Amazon Web Services SDK
  - Amazon EC2
  - CloudBees AWS Credentials (for storing amazon IAM credentials)
  - ansible
  
go to *manage jenkins* -> *global tool configuration* and place *"/usr/bin"* to ansible and *"git"* to git.
  
Finally create a cloud inside Jenkins:
  go to *manage jenkins* -> *configure system* and scroll all the way down. Here is some pictures to show how to create a cloud connection with jenkins:
  
  ![Image of mysql](https://github.com/SandorJokai/AWS-Ansible-Jenkins/blob/master/cloud-1.png)
  To get *amazon EC2 credentials* just go to AWS console and click on *my security credentials* under the username and click on *create new keys*
  
  ![Image of mysql](https://github.com/SandorJokai/AWS-Ansible-Jenkins/blob/master/cloud-2.png)
  Choose *us-east-1* as default and the private key is the one which might be dowloaded already. (*as the last process of launching an EC2*)
  Here is it in my case: [Private key](https://github.com/SandorJokai/AWS-Ansible-Jenkins/blob/master/keyForSSH.pem)\
  Choose an AMI ID which is Ubuntu 20.04 LTS in my case. (That is going to be the ansible worker node as well as the music-streamer server ultimately)
  
  ![Image of mysql](https://github.com/SandorJokai/AWS-Ansible-Jenkins/blob/master/cloud-3.png)
  Note there *T2 micro* has choosen. Make sure this must be chosen as it is eligible for free tier.
  Add some more parameters, choose Unix as AMI type where sudo is the *root command prefix* and 22 is the port number.
  *Idle termination time* is 30 minutes as default, which means the server will be terminated after 30 minutes idle.
  
  let's put some init script in order to work with the Jenkinsfile later on:
  
  apt update\
  apt install apache2 -y\
  apt install openjdk-8-jre -y
  
Note: there is no need to initialize with *#!/bin/bash* at beginning.
  
Click on *Advanced...* under the script field and choose 1 at *Minimum number of instances*. As result, one EC2 instance will be created automatically by these configurations. To check go to *manage jenkins* -> *manage nodes and clouds*.
  
Note: It is probably gonna take around 5 minutes until the ssh connection will be established. Wait for that, no rush... We can check the log when it's done anyway.

<h2>Create a job</h2>
  
We almost done. Let's create a job, choose *pipeline*, click on *pipeline* and add the Jenkinsfile from [here](https://github.com/SandorJokai/AWS-Ansible-Jenkins/blob/master/Jenkinsfile)
  
Don't forget to change the worker node's IP (music-streamer) in [dev.inv](https://github.com/SandorJokai/AWS-Ansible-Jenkins/blob/master/dev.inv)
  
Now let's jump into Jenkins-Ansible server via ssh and execute these commands:
  
```bash
mkdir /tmp/jenkins
```
```bash
cd /tmp/jenkins
```
```bash
git init
```
```bash
git pull https://github.com/SandorJokai/AWS-Ansible-Jenkins
```
```bash
chmod 400 keyForSSH.pem
```
```bash
sudo vi /etc/ansible/hosts
```
```bash
music-streamer ansible_host=172.31.*.* ansible_port=22 ansible_user=ubuntu
```
Note that *ansible_host* must be changed.
```bash
sftp -i keyForSSH.pem ec2-user@<Public-IP-Address>:/tmp/jenkins
```
Let's put some music into /tmp/jenkins
  
Let's check the connection finally:
```bash
ansible music-streamer --private-key keyForSSH.pem -m ping
```
  
It must be return with a green coloured message telling *"The connection is SUCCESS"*.
  
Once is done, build the pipeline finally.

![npm package](https://img.shields.io/badge/ansible-2.9.20-black.svg)
![npm package](https://img.shields.io/badge/python-2.7.18-turquoise.svg)
![npm package](https://img.shields.io/badge/git-2.23.4-red.svg)
![npm package](https://img.shields.io/badge/jenkins-2.289.1-purple.svg)
![npm package](https://img.shields.io/badge/amazon-aws-yellow.svg)
