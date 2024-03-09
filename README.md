# In this project, I will deploy a NodeJS application to an EC2 instance and Docker container in various ways.   
Below is the Github repository link.  

```
https://github.com/dhruv14385/node-todo-cicd
```
I will use EC2 Ubuntu instance.  
## Deployment option 1: On EC2 instance
•	We can simply deploy the app on EC2 without any integration to Jenkins.
•	Launch an EC2 Ubuntu instance. Make sure that the SG have port 8000 open to your IP. Connect using EC2 instance connect. 
•	Get updates.
sudo apt update 
•	Clone the repository
git clone https://github.com/dhruv14385/node-todo-cicd.git
•	Go to directory ‘node-todo-cicd’
cd node-todo-cicd
•	Install NodeJS on EC2
sudo apt install nodejs
sudo apt install npm
sudo npm install
•	Run the app
node app.js
•	Open browser and type <EC2-Public-IPv4>:8000. You should see the app running like below.
 
•	Go to instance connect and press Ctrl+C to come back to command prompt. The app will stop working. We can now go to option 2. Keep the instance running.
Deployment option 2: On EC2 instance by integrating code with Jenkins
•	Install Java version 11
sudo apt install openjdk-11-jre
•	Open port 8080 to your IP on SG.
•	Install Jenkins. Enter following commands one after other. 
o	sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
 https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
o	echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
o	sudo apt-get update
o	sudo apt-get install jenkins
o	sudo systemctl enable jenkins
o	sudo systemctl start jenkins
•	Confirm that Jenkins is running with following command.
sudo systemctl status Jenkins
•	Open browser and open Jenkins by typing <EC2-Public-IPv4>:8080. You should see the screen like below.
 
•	Check and copy paste the password in the file shown in red letters
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
•	Install suggested plugins 
•	Create first admin user by filling in the form
•	On Jenkins Dashboard, click on ‘Create a job’. 
 
•	On the next screen, name your job and select Freestyle project. After hitting OK, you will be redirected to Configure page. Add some description about the job. Select ‘GitHub project’ and enter project URL as below.
https://github.com/dhruv14385/node-todo-cicd
•	Scroll down and under ‘Source Code Management’, select ‘Git’ and enter Repository URL as below.
https://github.com/dhruv14385/node-todo-cicd.git
•	Under ‘Credentials’, click Add and then Jenkins.
 
•	Go to terminal and generate SSH keys.
•	Go to Github settings 
 
•	Copy the public key from terminal and paste it here.
 
•	Copy private key from terminal and fill up information and paste it as shown in the screenshots below.
 
 
•	Save the changes and click on ‘Build Now’ to build the code. It should be successful. You can check Console Output. Go to terminal and you should see the repository would be cloned in the directory highlighted in the screenshot below.
 
•	Install NodeJS in this directory and run the app as mentioned before. You should be able to see the app as shown in the last screenshot. 
•	Now to check whether the code is integrated or not properly, we will do some minor change in the code. Go to following file in Github in the repository.
views/todo.ejs
•	In line 92, where it is written 
<h1>Todo List - Made using Node</h1>
Change it to something like
<h1>Todo List - Made using Node by a Devops Engineer</h1>
and commit changes.
•	Build the code in Jenkins again and refresh the app in the browser. You will see the changes have taken place.
 
•	Hence it is proven that code has been integrated with Jenkins.


Deployment option 3: Docker
•	Install Docker
sudo apt install docker.io
•	Change your directory to ‘node-todo-cicd’ and build image from Dockerfile within that directory. This Dockerfile has base image of NodeJS. It exposes port 8000 and run the app when you will create a container from it.
sudo docker build . -t todo-note-app
•	Create a container from the image
sudo docker run -d --name node-todo-app -p 8000:8000 todo-note-app
•	Open browser and type <EC2-Public-IPv4>:8000 and you should see the app running, this time on a Docker container.
•	Stop this container before moving on to next option.
sudo docker kill <container ID>
Deployment option 4: Continuous Deployment on Docker container through Jenkins 
•	Now, for Continuous Deployment, as a first step, install a plugin called ‘GitHub Integration’ on Jenkins.
•	Go to repository settings and add a Webhook with following configuration.
 
•	And confirm that it working by checking the green tick. You may need to restart Jenkins.
 
•	In Jenkins, configure the project with following Build trigger
 
•	Enter following commands in the build steps section and save configuration.
 
•	Go to terminal and give permission to Jenkins to connect to docker
sudo usermod -aG docker Jenkins
•	Restart Jenkins from terminal
sudo systemctl restart Jenkins
•	Logout and Login Jenkins in the browser.
•	Now make any changes to the code in line 92 and commit changes. This should trigger build in Jenkins and container should be built. After successful build, you can check the app in the browser.
•	You may need to change name of the container and image in the ‘Build steps’ configuration of the project as shown in the screenshot above if you are getting error regarding that.
