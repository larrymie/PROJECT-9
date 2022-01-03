# PROJECT-9
TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. INTRODUCTION TO JENKINS


In previous Project 8 I configured horizontal scalability concept, which allow me to add new Web Servers to my tooling Website and i have successfully deployed a set up with 2 Web Servers and also a Load Balancer to distribute traffic between them. If it is just two or three servers – it is not a big deal to configure them manually. Imagine that I have to repeat the same task over and over again adding dozens or even hundreds of servers.

In this project I am going to start automating part of our routine tasks with a free and open source automation server – Jenkins.

In this project I am going to utilize Jenkins CI capabilities to make sure that every change made to the source code in GitHub https://github.com/<yourname>/tooling will be automatically be updated to the Tooling Website.

#Task#
I will enhance the architecture prepared in Project 8 by adding a Jenkins server, I will configure a job to automatically deploy source codes changes from Git to NFS server.

Here is how my updated architecture will look like upon completion of this project: IMAGE 01

![01](https://user-images.githubusercontent.com/91284177/147443870-c54509e3-5716-4325-a5e8-3245519502e3.png)

I Created an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"

#INSTALL AND CONFIGURE JENKINS SERVER#
Step 1 – I Installed Jenkins server (since Jenkins is a Java-based application) using;
<sudo apt update
sudo apt install default-jdk-headle>

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins

I confirmed that Jenkins is up and running. IMAGE 02

![02](https://user-images.githubusercontent.com/91284177/147446195-5eaf03df-e321-4aa9-829f-83359618d27f.png)

By default Jenkins server uses TCP port 8080 – I opened it by creating a new Inbound Rule in my EC2 Security Group.

I Performed the initial Jenkins setup. IMAGES 3-9.


![03](https://user-images.githubusercontent.com/91284177/147448315-78c5dde5-8b5d-449e-aeda-6d2f8f6c5d33.png)
![04](https://user-images.githubusercontent.com/91284177/147448325-5b8df6e6-a4d0-452d-a9ce-b214348d3b4c.png)
![05](https://user-images.githubusercontent.com/91284177/147448336-09c120fb-fb55-43b0-a6e0-faab7b3fe018.png)
![06](https://user-images.githubusercontent.com/91284177/147448347-60595010-8a03-4ce9-b15e-074e4fab4bd2.png)
![07](https://user-images.githubusercontent.com/91284177/147448352-fa0f750b-141e-4b50-9a56-c7449982bbfe.png)
![08](https://user-images.githubusercontent.com/91284177/147448365-37ea55ba-77ff-46de-b7a2-54ecd869ac6f.png)
![09](https://user-images.githubusercontent.com/91284177/147448373-c6a4c448-5d4b-4c43-a6c5-8c1ae61cbcb1.png)


#Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks#

This job will will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

I enabled webhooks in my GitHub repository settings. IMAGE 10
![10](https://user-images.githubusercontent.com/91284177/147449820-7dbd1910-0175-46f3-86e6-f134adb47466.png)

On my Jenkins web console, I clicked "New Item" and create a "Freestyle project" IMAGES 11-14.

![11](https://user-images.githubusercontent.com/91284177/147465347-ee683925-3e55-4934-b68e-06e0180b07d3.png)
![12](https://user-images.githubusercontent.com/91284177/147465351-e2656491-20db-46ec-968e-81b7e5941712.png)
![13](https://user-images.githubusercontent.com/91284177/147465361-068e4d5b-4768-4d71-b66b-c58709970d1e.png)
![14](https://user-images.githubusercontent.com/91284177/147465376-3790f349-2205-4bbc-8152-c09ef7cab05e.png)
  
The above only run manually. I changed it to automation by Clicking "Configure" your job/project and added these two configurations. IMAGES 15 & 16

![15](https://user-images.githubusercontent.com/91284177/147465819-e0516eee-0003-4c88-87e5-1f15a77f8f6d.png)
![16](https://user-images.githubusercontent.com/91284177/147465828-646e17b3-b297-4d77-8ed5-f228ac396bf0.png)

I changed a code in README.MD file in my GitHub Tooling repository and Webhook automatically triggered a new job in the "Console Output" IMAGE 17
![17](https://user-images.githubusercontent.com/91284177/147958953-533470a7-a967-453e-a839-9429c6b016aa.png)

I also connected via SSH/Putty to my NFS server to check README.MD file. IMAGE18
![18](https://user-images.githubusercontent.com/91284177/147959090-399cc5ed-c255-4eb3-8916-a74310c54ee0.png)

