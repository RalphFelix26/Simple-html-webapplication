# Simple-html-webapplication

This is a basic example of a web application that demonstrates a simple static HTML page with basic interactivity. Below is the code and deployment process to set up and scale this application using AWS EC2, ALB, and Auto Scaling.

## Table of Contents
1. [HTML Application Code](#html-application-code)
2. [CSS for Styling](#css-for-styling)
3. [Deployment on AWS EC2](#deployment-on-aws-ec2)
4. [Load Balancer and Auto Scaling](#load-balancer-and-auto-scaling)
5. [SNS Notifications](#sns-notifications)
6. [Infrastructure Automation](#infrastructure-automation)
7. [Testing the Setup](#testing-the-setup)
8. [Optional Enhancements](#optional-enhancements)

---

## HTML Application Code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Web Application</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Welcome to My Simple Web Application!</h1>
        <p>This is a basic web application hosted on AWS EC2.</p>
        <button id="clickButton">Click Me!</button>
        <p id="responseText"></p>
    </div>
    <script>
        document.getElementById("clickButton").addEventListener("click", function() {
            document.getElementById("responseText").textContent = "You clicked the button!";
        });
    </script>
</body>
</html>
CSS for Styling
css
Copy code
/* Basic styling for the simple HTML application */
body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    margin: 0;
    padding: 0;
}
.container {
    max-width: 800px;
    margin: 0 auto;
    padding: 50px;
    text-align: center;
    background-color: #ffffff;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
}
h1 {
    color: #333;
}
button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    margin-top: 20px;
    background-color: #007bff;
    color: #ffffff;
    border: none;
    border-radius: 5px;
}
button:hover {
    background-color: #0056b3;
}
#responseText {
    margin-top: 20px;
    font-weight: bold;
    color: #007bff;
}
Deployment on AWS EC2
1. Create EC2 Instances
Log in to the AWS Management Console.
Launch two EC2 instances using an Ubuntu 20.04 AMI.
Configure the security group to open HTTP (port 80) and SSH (port 22).
SSH into the instance:
bash
Copy code
ssh -i "your-key.pem" ubuntu@<your-ec2-public-ip>
2. Configure the Instance
Install Nginx:
bash
Copy code
sudo apt update
sudo apt install nginx -y
Upload the HTML files:
bash
Copy code
scp -i "your-key.pem" index.html styles.css ubuntu@<your-ec2-public-ip>:/home/ubuntu/
Move the files to the Nginx directory:
bash
Copy code
sudo mv /home/ubuntu/index.html /var/www/html/
sudo mv /home/ubuntu/styles.css /var/www/html/
Restart Nginx:
bash
Copy code
sudo systemctl restart nginx
Load Balancer and Auto Scaling
1. Set Up Load Balancer
Create an Application Load Balancer (ALB) and target group.
Register EC2 instances to the target group.
2. Configure Auto Scaling
Create an Auto Scaling group with scaling policies (e.g., scale up when CPU > 80%).
SNS Notifications
Create an SNS Topic for notifications.
Subscribe your email or phone number to the topic.
Infrastructure Automation
Automate the entire setup using a Python script with Boto3:

python
Copy code
import boto3

ec2 = boto3.client('ec2')
alb = boto3.client('elbv2')
asg = boto3.client('autoscaling')
sns = boto3.client('sns')

def deploy_infrastructure():
    # Add infrastructure setup steps using Boto3

if __name__ == "__main__":
    deploy_infrastructure()
Testing the Setup
Access the application via the ALB DNS name.
Simulate traffic spikes to test Auto Scaling.
Verify that SNS notifications are received for scaling events.
Optional Enhancements
Use S3 to store static files.
Create Lambda functions for dynamic tasks such as moving files between services.
Conclusion
By following this guide, you will set up a scalable and automated web application infrastructure on AWS, gaining hands-on experience with EC2, ALB, Auto Scaling, SNS, and automation tools like Boto3.
