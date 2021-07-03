![npm package](https://img.shields.io/badge/ansible-2.9.20-black.svg)
![npm package](https://img.shields.io/badge/python-2.7.18-turquoise.svg)
![npm package](https://img.shields.io/badge/git-2.23.4-red.svg)
![npm package](https://img.shields.io/badge/jenkins-2.289.1-purple.svg)
![npm package](https://img.shields.io/badge/amazon-aws-yellow.svg)

<h2>Introducing the project</h2>

Here is another nice combination of my favourite tools, AWS, Ansible and Jenkins. In this case, once we launched an EC2 instance from AWS (that is going to be the Ansible controller as well as the Jenkins server) and make some settings, a fully CI/CD pipeline will be created demonstrated the Open source music-streamer provided by Ampache.

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
  
Once we logged in with typing <Public-IP-Address:8080> grab the initial admin password from here:
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
  
go to *manage jenkins* -> *global tool configuration* and place *"/usr/bin"* to git and ansible.
  
Finally create a cloud inside Jenkins:
  go to *manage jenkins* -> *configure system* and scroll all the way down. Here is some pictures to show how to create a cloud connection with jenkins:
  
  ![Image of mysql](https://github.com/SandorJokai/AWS-Ansible-Jenkins/blob/master/cloud-1.png)
  ![Image of mysql](https://github.com/SandorJokai/AWS-Ansible-Jenkins/blob/master/cloud-2.png)
  ![Image of mysql](https://github.com/SandorJokai/AWS-Ansible-Jenkins/blob/master/cloud-3.png)
