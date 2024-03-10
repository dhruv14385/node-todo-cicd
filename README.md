# CI/CD using Jenkins  
## In this project, I will deploy a NodeJS application to an EC2 instance and a Docker container in various ways.   
Below is the Github repository link.  

```
https://github.com/dhruv14385/node-todo-cicd
```
I will use EC2 Ubuntu instance.  
## Deployment option 1: On EC2 instance
•	We can simply deploy the app on EC2 without any integration to Jenkins.  
•	Launch an EC2 Ubuntu instance. Make sure that the SG have port 8000 open to your IP. Connect using EC2 instance connect.   
•	Get updates.  
```
sudo apt update
```
•	Clone the repository  
```
git clone https://github.com/dhruv14385/node-todo-cicd.git
```
•	Go to directory ‘node-todo-cicd’  
```
cd node-todo-cicd
```
•	Install NodeJS on EC2  
```
sudo apt install nodejs
sudo apt install npm
sudo npm install
```
•	Run the app  
```
node app.js
```
•	Open browser and type (EC2-Public-IPv4):8000. You should see the app running like below.   

![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/e006b4b2-0c80-4aee-866f-b5c2d64e9e81)

 
•	Go to instance connect and press Ctrl+C to come back to command prompt. The app will stop working. We can now go to option 2. Keep the instance running.  
## Deployment option 2: On EC2 instance by integrating code with Jenkins  
•	Install Java version 11  
```
sudo apt install openjdk-11-jre
```

•	Open port 8080 to your IP on SG.  
•	Install Jenkins. Enter following commands one after other.   
```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
 https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```
```
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
```
```
sudo apt-get update
sudo apt-get install jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
```
•	Confirm that Jenkins is running with following command.  
```
sudo systemctl status Jenkins
```
•	Open browser and open Jenkins by typing (EC2-Public-IPv4):8080. You should see the screen like below.  
![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/2f540520-fb79-4ae0-bc3e-1e6ca3869c11)  
 
•	Check and copy paste the password in the file shown in red letters  
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
•	Install suggested plugins   
•	Create first admin user by filling in the form
•	On Jenkins Dashboard, click on ‘Create a job’.   

![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/1bc5b3eb-8788-46e8-82b4-e847ada57b0b)  
 
•	On the next screen, name your job and select Freestyle project. After hitting OK, you will be redirected to Configure page. Add some description about the job. Select ‘GitHub project’ and enter project URL as below.  
```
https://github.com/dhruv14385/node-todo-cicd
```
•	Scroll down and under ‘Source Code Management’, select ‘Git’ and enter Repository URL as below.  
```
https://github.com/dhruv14385/node-todo-cicd.git
```
•	Under ‘Credentials’, click Add and then Jenkins.  

![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/70bc9b35-3e7a-4b8e-86f6-6e1ce3015df1)  

•	Go to terminal and generate SSH keys.  
•	Go to Github settings 

![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/5508f4f0-5d0c-4909-80c0-8b4cef0394c4)  

•	Copy the public key from terminal and paste it here.

![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/a2eccac9-5e68-4688-9d7b-d832bef9433d)  
 
•	Copy private key from terminal and fill up information and paste it as shown in the screenshots below.  

![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/68daa494-31d3-41c5-a9e4-6acd9523370a)   
![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/4dddc557-4f95-4261-a1b4-d187e3f3b83d)  
 
•	Save the changes and click on ‘Build Now’ to build the code. It should be successful. You can check Console Output. Go to terminal and you should see the repository would be cloned in the directory highlighted in the screenshot below.  

![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/f9191d08-7c5b-4494-8195-1ec4b24f41f3)  
 
•	Install NodeJS in this directory and run the app as mentioned before. You should be able to see the app as shown in the last screenshot.   
•	Now to check whether the code is integrated or not properly, we will do some minor change in the code. Go to following file in Github in the repository.   
views/todo.ejs  
•	In line 92, where it is written   
&lt;h1&gt;Todo List - Made using Node&lt;h1&gt;  
Change it to something like  
&lt;h1&gt;Todo List - Made using Node by a Devops Engineer&lt;h1&gt;    
and commit changes.  
•	Build the code in Jenkins again and refresh the app in the browser. You will see the changes have taken place.

![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/91e3f3fe-8d83-4956-94e0-9e2b3e0f7bf7)  
  
•	Hence it is proven that code has been integrated with Jenkins.  


## Deployment option 3: Docker  
•	Install Docker  
```
sudo apt install docker.io
```
•	Change your directory to ‘node-todo-cicd’ and build image from Dockerfile within that directory. This Dockerfile has base image of NodeJS. It exposes port 8000 and run the app when you will create a container from it.  
```
sudo docker build . -t todo-note-app
```
•	Create a container from the image
```
sudo docker run -d --name node-todo-app -p 8000:8000 todo-note-app
```
•	Open browser and type &lt;EC2-Public-IPv4&gt;:8000 and you should see the app running, this time on a Docker container.  
•	Stop this container before moving on to next option.  
```
sudo docker kill <container ID>
```
## Deployment option 4: Continuous Deployment on Docker container through Jenkins  
•	Now, for Continuous Deployment, as a first step, install a plugin called ‘GitHub Integration’ on Jenkins.  
•	Go to repository settings and add a Webhook with following configuration.  
![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/c6660aa8-1727-40f1-859f-225057326149)  
 
•	And confirm that it working by checking the green tick. You may need to restart Jenkins.  
![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/f1ce81fd-6db1-4cfb-978c-21f29830a778)  
 
•	In Jenkins, configure the project with following Build trigger  
![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/f34fa7a4-8681-4279-b769-aa942333a654)  
 
•	Enter following commands in the build steps section and save configuration.  
![image](https://github.com/dhruv14385/node-todo-cicd/assets/83332524/505179c5-3f56-42bc-99f7-fd2b9c44f8ee)  
 
•	Go to terminal and give permission to Jenkins to connect to docker  
```
sudo usermod -aG docker Jenkins
```
•	Restart Jenkins from terminal  
```
sudo systemctl restart Jenkins
```
•	Logout and Login Jenkins in the browser.  
•	Now make any changes to the code in line 92 and commit changes. This should trigger build in Jenkins and container should be built. After successful build, you can check the app in the browser.  
•	You may need to change name of the container and image in the ‘Build steps’ configuration of the project as shown in the screenshot above if you are getting error regarding that.  
