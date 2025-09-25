# Jenkins-skeleton-projects
Install Jenkins, configure Docker as agent, set up cicd, deploy applications to k8s and much more.

AWS EC2 Instance
Go to AWS Console
Instances(running)
Launch instances

<img width="1214" height="863" alt="image" src="https://github.com/user-attachments/assets/877b3dd4-5419-4642-85de-e0906ac59ddf" />


Install Jenkins.
Pre-Requisites:

Java (JDK)
Run the below commands to install Java and Jenkins
Install Java

sudo apt update
sudo apt install openjdk-17-jre
Verify Java is Installed

java -version
Now, you can proceed with installing Jenkins

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Open port 8080 in the inbound traffic rules as show below.

EC2 > Instances > Click on
In the bottom tabs -> Click on Security
Security groups
Add inbound traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed All traffic).
<img width="1598" height="284" alt="image" src="https://github.com/user-attachments/assets/31d52e81-76d4-42c0-a150-489a9a1432c5" />


Login to Jenkins using the below URL:
http://:8080 [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]

Note: If you are not interested in allowing All Traffic to your EC2 instance 1. Delete the inbound traffic rule for your instance 2. Edit the inbound traffic rule to only allow custom TCP port 8080

After you login to Jenkins, - Run the command to copy the Jenkins Admin Password - sudo cat /var/lib/jenkins/secrets/initialAdminPassword - Enter the Administrator password

<img width="888" height="378" alt="image" src="https://github.com/user-attachments/assets/af4dee5c-53b4-4030-92ca-7d1776eb642b" />


Click on Install suggested plugins
S<img width="895" height="445" alt="image" src="https://github.com/user-attachments/assets/a58e3266-5750-45aa-a4e3-a183f7ef6ffb" />


Wait for the Jenkins to Install suggested plugins

<img width="891" height="463" alt="image" src="https://github.com/user-attachments/assets/390ff81f-0a5a-4544-b9c4-92b7335f45ad" />


Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

<img width="882" height="520" alt="image" src="https://github.com/user-attachments/assets/4528522a-61b2-4e9c-81f9-e581edf2897a" />


Jenkins Installation is Successful. You can now starting using the Jenkins

<img width="878" height="487" alt="image" src="https://github.com/user-attachments/assets/b3fe28ab-c4a9-421e-97dc-5995cbe693c8" />


Install the Docker Pipeline plugin in Jenkins:
Log in to Jenkins.
Go to Manage Jenkins > Manage Plugins.
In the Available tab, search for "Docker Pipeline".
Select the plugin and click the Install button.
Restart Jenkins after the plugin is installed.
<img width="868" height="361" alt="image" src="https://github.com/user-attachments/assets/50dbc89f-7b05-4113-8e1e-e7254d893267" />


Wait for the Jenkins to be restarted.

Docker Slave Configuration
Run the below command to Install Docker

sudo apt update
sudo apt install docker.io
Grant Jenkins user and Ubuntu user permission to docker deamon.
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
Once you are done with the above steps, it is better to restart Jenkins.

http://<ec2-instance-public-ip>:8080/restart
The docker agent configuration is now successful.
