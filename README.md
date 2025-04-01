# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263) How to Deploy WordPress Website on AWS using EC2, RDS, ALB and more (Part1).

<div align="center">

  <br />
    <a href="https://youtu.be/FF7nJH2NeJc?si=OeDYVkMRR9174xHP" target="_blank">
      <img src="https://github.com/user-attachments/assets/39874a19-2b7d-4eb5-80d9-eb7e5256f8f7" alt="Project Banner">
    </a>
  <br />

<h3 align="center">How to Deploy WordPress Website on AWS using EC2</h3>

   <div align="center">
     Build this hands-on demo step by step with my detailed tutorial on <a href="http://www.youtube.com/@julienmuke/videos" target="_blank"><b>Julien Muke</b></a> YouTube. Feel free to subscribe 🔔!
    </div>
</div>

## 🚨 Tutorial

This repository contains the steps corresponding to an in-depth tutorial available on our YouTube
channel, <a href="http://www.youtube.com/@julienmuke/videos" target="_blank"><b>Julien Muke</b></a>.

If you prefer visual learning, this is the perfect resource for you. Follow my tutorial to learn how to build projects
like these step-by-step in a beginner-friendly manner!

<a href="https://youtu.be/FF7nJH2NeJc?si=F1tzYZpLitTtUOLT" target="_blank"><img src="https://github.com/sujatagunale/EasyRead/assets/151519281/1736fca5-a031-4854-8c09-bc110e3bc16d" /></a>

## <a name="introduction">🤖 Introduction</a>

We’re going to walk through how to deploy a React.js application on AWS using an EC2 instance with Terraform. If you want to automate your infrastructure and host your React app on the cloud, this is the video for you.

We’ll cover everything from setting up Terraform, launching an EC2 instance, and deploying your React app step-by-step. So, stick around, and by the end of this tutorial, you’ll have your React.js app live on AWS!


## 📐 Architecture Diagram Overview

![Image](https://github.com/user-attachments/assets/d389dae9-e6e7-44af-b15e-791d5f8f9642)


## <a name="steps">☑️ Steps</a>

1. Create an IAM User
2. Configure AWS profile and set up the AWS CLI
3. Create Terraform file (.tf) and write the Script
4. Execute the Terraform Script

## <a name="quick-start">🤸 Quick Start</a>

Follow these steps to set up the project locally on your machine.

**Prerequisites**

Make sure you have the following installed on your machine:

- The [Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) (1.2.0+) installed.
- The [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) installed.
- The [AWS account](https://aws.amazon.com/free/) and [associated credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html) that allow you to create resources.


## <a name="clone-repo">🚀 Cloning the Repository</a>

First, we’ll set up a React app by cloning the React app from GitHub. If you already have one, feel free to skip this part.

```bash
git clone https://github.com/julien-muke/brainwave.git
```

## ➡️ Step 1 - Create an IAM User, and the Access Key

First, we need to create a user and then create access keys for that particular user so that we can work with them for our authentication purpose.

1. Navigate to your AWS Console, search for AWS Identity and Access Management (IAM)
2. Go the Tab at righ hand side and choose "Users"
3. Click "Create user"

![Screenshot 2024-05-05 at 12 56 48](https://github.com/julien-muke/aws-ec2-terraform/assets/110755734/ded49b4d-5a14-4db0-8cc0-fabcbfcd3474)


4. Enter the "User name" i will use `ec2-terraform` then click "Next"

![Screenshot 2024-05-05 at 12 57 20](https://github.com/julien-muke/aws-ec2-terraform/assets/110755734/457b10b7-aa88-436c-9eb2-e1d99b2f9346)

5. Set permissions, choose "Attached policies directly" search and choose `AmazonEC2FullAccess` then click "Next"

![Screenshot 2024-05-05 at 12 58 56](https://github.com/julien-muke/aws-ec2-terraform/assets/110755734/30f06e43-5147-49ec-b22a-608add702318)


6. Review the details and click "Create user"

Now you can see that our user has been successfully created.

Next, let's create the access key for the user

7. Click on the user we just created

![Screenshot 2024-05-05 at 12 59 44](https://github.com/julien-muke/aws-ec2-terraform/assets/110755734/5c8a1851-c9d4-4500-9816-d913cf133e2c)


8. Scroll down to security crendentials tab, and click "Create access key"
9. Since I am going to work with command line interface, we will choose CLI and check the confirmation box and click "Next"

![Create-access-key-IAM-Global](https://github.com/julien-muke/aws-ec2-terraform/assets/110755734/38d247fc-4ed8-4e45-962c-c8c17400dbd3)

10. The access key is successfully created, now it will show the access key as well as the secret access key, I will simply go and download the CSV file.

![Screenshot 2024-05-05 at 13 00 36](https://github.com/julien-muke/aws-ec2-terraform/assets/110755734/0c21c652-5cf0-4b8f-8e8e-bfb828a03e2c)


## ➡️ Step 2 - Configure AWS profile and set up the AWS CLI

We are going to configure basic settings that the AWS Command Line Interface (AWS CLI) uses to interact with AWS. These include your security credentials, the default output format, and the default AWS Region.


1. Open VS Code (or any IDE of your choise) and run the command `aws configure`
2. Copy and paste your AWS Access Key ID
3. Copy and paste your AWS Secret Access key
4. Keep as default region name and output format

![1](https://github.com/julien-muke/aws-ec2-terraform/assets/110755734/ad38c212-ed4a-4730-9614-8a8959faab10)

5. To check if the AWS profile is configured run the following command `aws configure list`

![2](https://github.com/julien-muke/aws-ec2-terraform/assets/110755734/3d5a2a6f-9d22-4cf7-b065-3e5d2b0765c7)


## ➡️ Step 3 - Create Terraform file (.tf) and write the Script


1. Create a file to define your infrastructure by running the following command:

```bash
touch main.tf
```

2. Open main.tf in your text editor (VS Code), copy and paste the configuration below.

⚠️Note: This script provisions an EC2 instance, installs Nginx, and deploys your React app. 


```bash
provider "aws" {
  region = "us-east-1" # Change to your preferred region
}

resource "aws_instance" "react_app" {
  ami           = "ami-xxxxxxxxxxxx"  # Amazon Linux 2 AMI (Change based on your region)
  instance_type = "t2.micro"

  security_groups = [aws_security_group.react_sg.name]

  user_data = <<-EOF
             #!/bin/bash
              sudo yum update -y
              sudo amazon-linux-extras enable nginx1
              sudo yum install -y nginx
              sudo systemctl start nginx
              sudo systemctl enable nginx
              
              # Install Node.js and npm
              curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
              sudo yum install -y nodejs
              
              # Clone React App from GitHub
              sudo yum install -y git
              git clone https://github.com/yourusername/your-react-app.git /home/ec2-user/react-app
              
              # Build React App
              cd /home/ec2-user/react-app
              npm install
              npm run build
              
              # Remove default Nginx welcome page
              sudo rm -rf /usr/share/nginx/html/*
              
              # Copy React build files to Nginx
              sudo cp -r /home/ec2-user/react-app/build/* /usr/share/nginx/html/

              # Restart Nginx
              sudo systemctl restart nginx
              EOF

  tags = {
    Name = "ReactAppServer"
  }
}

resource "aws_security_group" "react_sg" {
  name        = "react_app_sg"
  description = "Allow HTTP and SSH access"

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```