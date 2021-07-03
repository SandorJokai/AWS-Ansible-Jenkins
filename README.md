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
  
Now the Jenkins is up and running. 
