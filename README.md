# Deploy single/multiple Node.js apps on AWS EC2 instance with live domains, Nginx.

## Introduction

### Node.js

Node.js is an open-source, cross-platform, JavaScript runtime environment that executes JavaScript code outside a web browser. Node.js is built on open-source V8 JavaScript engine for easily building fast and scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.

### AWS

AWS is a platform that offers flexible, reliable, scalable, easy-to-use and cost-effective cloud computing solutions and is offered by Amazon. The platform is developed with a combination of infrastructure as a service (IaaS), platform as a service (PaaS) and packaged software as a service (SaaS) offerings.

### EC2

EC2 or Elastic Compute Cloud is a web service that provides secure, resizable compute capacity in the cloud. Amazon EC2’s simple web service interface allows you to obtain and configure capacity with minimal friction. It provides you with complete control of your computing resources and lets you run on Amazon’s proven computing environment.

### Cloudflare

Cloudflare is an American web-infrastructure and website-security company, providing content-delivery-network services, DDoS mitigation, Internet security, and distributed domain-name-server services.

### Git

Git is a distributed version control system for tracking changes in source code during software development.


## What we are going to implement

1. What we are going to implement
2. Create an AWS account
3. Launch an Ubuntu EC2 instance
4. SSH into our instance
5. Install Node.js and Git
6. Clone repository from GitHub
7. Run the node.js app
8. Open different ports for the node instances
9. Make the app running using PM2
10. Configure Nginx for single and multiple node.js apps
11. Configure Nginx to autostart whenever instance will be rebooted
12. Purchase a domain from Godaddy, Namecheap, etc.
13. Configure domain name for our server
14. Configure subdomains for our server
15. Setup SSL with Cloudflare

### create an AWS Account

We are using AWS EC2 server for hosting our multiple node.js apps. So, It is must to have an AWS account. Let’s start and create if you are not having one. For new users, there is an offer of 750 hours free per month up to a year. You must have a credit/debit card before start registering.

### Note: If you are already having an AWS account then you can skip this step and just login to your instance.

1. Open [AWS](https://aws.amazon.com) and Click on Create an AWS account.
2. After this, you will be asked to choose a support plan. Choose a basic plan which is free or choose another one if you want extra features.
3. Now you’ve successfully created your account. Now simply log in to the AWS console.

### Launch an Ubuntu EC2 instance 

Now to host our node.js apps we need an **EC2 virtual machine or instance**. So let’s get started to make our first EC2 instance up.

1. Log in to [AWS](https://aws.amazon.com) and click on the Sign in to the Console. Open your AWS console.
2. Click on EC2 in the All Services section.

![AWS console image](https://user-images.githubusercontent.com/98725622/221651498-c929493c-92b0-49d3-807e-c4ea7b948f33.png)


3. You will see a screen like below. Where all the currently running instances can be shown. Now click on the Running instances.

### Note:  Right now I am having 1 instances and other services running so it is displaying there. But if you are new then there are 0 displaying.

![AWS EC2 instance image](https://user-images.githubusercontent.com/98725622/221651787-4f2b670f-2e53-4ec3-b840-bb2f153ffbd7.png)

4. After clicking on Running Instances you’ll see a Launch Instance button there.

5. Now, click on the Launch Instance button. After clicking you will see a page like this which shows the type of OS with the config you want in your machine e.i. We are going to use a Ubuntu Server 22.04 LTS 64-bit (x86).

![AWS Launch instance image](https://user-images.githubusercontent.com/98725622/221650590-5aa7487f-983a-4155-a48e-e453b7bd769a.png)

![AWS console image](./assets/instance-type.png)

6. create new key pair if you are not already have. give a name of key and select format of key if you are windows user and connect your server using putty then select .ppk type or if you wish to use with open ssh then choose .pem type.

![AWS console image](./assets/key-pair.png)

6. In advance details select Shutdown behavior to stop

![AWS console image](./assets/shut-down-behavior.png)

7. click on launch instance button

![AWS console image](./assets/launch-log.png)

8. Now we successfully launch our instance and click on  view all instances.

9. After instance initialization, select the instance and click on connect. Then we can easily connect to our instance in browser, on Mac terminal by .pem file, and on Windows by using putty.

### Note: Keep your key in a secure place and don’t share your key with someone else not working with you on this.

Your key must not be publicly viewable for SSH to work. Use the below command if needed. Always provide a correct path for the key. In below example, I am in the same directory in which my pen file or my key is present.

chmod 400 path-for-key-here
For Example:
chmod 400 ./node-key.pem

10. 9. Now click on the instance id ( In my case i-0aaa83ad977631403 ) which appears in the green block above. You will be redirected to the instances page and now you can see your instance is up and running.

![AWS all instances](./assets/launch-log.png)

## SSH into our instance

SSH means Secure Shell. The purpose of doing SSH to our instance is we want to access our server through a secure manner which you can access by using your terminal/cmd or using other Softwares like Putty. Now, depending upon your Operating System you can access your server in different manners.

### For Windows

You can use Putty, etc.

## For Mac / Linux

- Open your terminal
- Go to AWS EC2 instances section and choose any instance.

![Public DNS ]

- Copy the Public IP and write the below command in your terminal with your instance public IP which you have copied.

Note: Please make sure your private key location is correct in your local PC. Which you have downloaded in the previous step. In below example, I am in the same directory in which the node-key.pem is present.

ssh -i "private-key-location" ubuntu@public-ip
For example
ssh -i "./node-key.pem" ubuntu@ec2-54-161-91-174.compute-1.amazonaws.com

## In windows connect with Putty

download and Install putty then open it.

For generate .ppk file from .pem file that are downloaded from aws refer this link

Please refer to the following link for instructions on generating a .ppk file from a .pem file that was downloaded from Amazon Web Services.

https://asf.alaska.edu/how-to/data-recipes/connect-to-your-ec2-instance-using-putty-v1-1/

After generate .ppk file follow this instruction for connect your aws instance with putty

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html

![Putty server connection ]

## Install Node.js and Git
### Installing Node :

To run a Node.js app we require Node.js to be installed on our instance. So we will install all the required dependencies which can be used to install Node.js on our ubuntu instance.

There are many ways to install node on our instance:

Installing Node.js and npm from NodeSource

First, of all Enable the NodeSource repository by running the following curl command as a sudo user on your instance using the terminal:

Here is the full documentation of how to install node.js in Ubuntu. You can install node.js on your server by following these instructions.

https://github.com/nodesource/distributions/blob/master/README.md

OR

https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04

OR

Run this command to install node.js

curl -fsSL https://deb.nodesource.com/setup_19.x | sudo -E bash - &&\
sudo apt-get install -y nodejs

Here you can change node version by providing setup number like setup_14.x in above command

Now, Verify that the Node.js and npm were successfully installed by printing their versions:

$ node --version
Output: v19.7.0
$ npm --version
Output: 9.5.0

![Node version check]

## Installing Git :

To get our code on our EC2 Instance we need git. So, the below steps will define how to install Git on our EC2 instance.

To refresh all the packages run

$ sudo apt update -y

![Git install image]

### Clone repository from GitHub

Now, we have Node.js and Git installed on our EC2 Instance. Now, we need the codebase on our ec2 instance. So, we will clone our repositories using the username and password from services like Github, Bitbucket, Gitlab, etc. I am using Github here.

Go to Github.
Navigate to your repository which you want to clone.
Click on Code.