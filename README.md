# ![aws](https://github.com/julien-muke/Search-Engine-Website-using-AWS/assets/110755734/01cd6124-8014-4baa-a5fe-bd227844d263) Deploy a React.js application on AWS using an EC2 instance with Terraform

<div align="center">

  <br />
    <a href="https://youtu.be/FF7nJH2NeJc?si=OeDYVkMRR9174xHP" target="_blank">
      <img src="https://github.com/user-attachments/assets/e8f01153-2ea4-4c89-853f-1b604b6effad" alt="Project Banner">
    </a>
  <br />

<h3 align="center">Deploying a React app using EC2 and Terraform</h3>

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

We’re going to walk through how to deploy a React.js application on AWS using an EC2 instance with Terraform. If you want to automate your infrastructure and host your React app on the cloud, this is the tutorial for you.

We’ll cover everything from setting up Terraform, launching an EC2 instance, and deploying your React app step-by-step. By the end of this tutorial, you’ll have your React.js app live on AWS!


## 📐 Architecture Diagram

![Image](https://github.com/user-attachments/assets/d389dae9-e6e7-44af-b15e-791d5f8f9642)


## <a name="steps">☑️ Steps</a>

1. Create an IAM User
2. Configure AWS profile and set up the AWS CLI
3. Create Terraform file (.tf) and write the Script
4. Execute the Terraform Script

## <a name="quick-start">🤸 Quick Start</a>

**Prerequisites**

Make sure you have the following installed on your machine:

- [Node.js](https://nodejs.org/en) installed on your system.
- [Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) (1.2.0+) installed.
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) installed.
- [AWS account](https://aws.amazon.com/free/) and [associated credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/security-creds.html) that allow you to create resources.


## <a name="clone-repo">	:octocat:  Cloning the Repository</a>

First, we’ll set up a React app by cloning the React app from GitHub. If you already have one, feel free to skip this part.

```bash
git clone https://github.com/julien-muke/brainwave.git
```

## ➡️ Step 1 - Create an IAM User, and the Access Key

First, we need to create a user and then create access keys for that particular user so that we can work with it for our authentication purpose.

1. Navigate to your AWS Console, search for AWS Identity and Access Management (IAM)
2. Go the Tab at righ hand side and choose "Users"
3. Click "Create user"

![Image](https://github.com/user-attachments/assets/71d0fb34-ca51-4b62-b80d-6e78c7f31451)

4. Enter the "User name" i will use `ec2-terraform` then click "Next"

![Image](https://github.com/user-attachments/assets/45c24e90-faa2-4728-8e0d-7a24a023d953)
5. Set permissions, choose "Attached policies directly" search and choose `AmazonEC2FullAccess` then click "Next"

![Image](https://github.com/user-attachments/assets/a78f2a07-39b3-4959-8df6-2c8646fee142)

6. Review the details and click "Create user"

Now you can see that our user has been successfully created.

Next, let's create the access key for the user

7. Click on the user we just created

![Image](https://github.com/user-attachments/assets/5856a2e0-2e85-4b3a-9d40-34f259c36248)

8. Scroll down to security crendentials tab, and click "Create access key"
9. Since I am going to work with command line interface, we will choose CLI and check the confirmation box and click "Next"

![Image](https://github.com/user-attachments/assets/a2737aa2-2ae9-416f-8744-2594cc7ca205)

10. The access key is successfully created, now it will show the access key as well as the secret access key, I will simply go and download the CSV file.

![Image](https://github.com/user-attachments/assets/a9679222-d84f-4855-a0d5-869667a8dc6d)


## ➡️ Step 2 - Configure AWS profile and set up the AWS CLI

We are going to configure basic settings that the AWS Command Line Interface (AWS CLI) uses to interact with AWS. These include your security credentials, the default output format, and the default AWS Region.


1. Open VS Code (or any IDE of your choise) and run the command `aws configure`
2. Copy and paste your AWS Access Key ID
3. Copy and paste your AWS Secret Access key
4. Keep as default region name and output format

![1](https://github.com/julien-muke/aws-ec2-terraform/assets/110755734/ad38c212-ed4a-4730-9614-8a8959faab10)

5. To check if the AWS profile is configured run the following command `aws configure list`

![2](https://github.com/julien-muke/aws-ec2-terraform/assets/110755734/3d5a2a6f-9d22-4cf7-b065-3e5d2b0765c7)


## ➡️ Step 3 - Set Up Terraform Configuration

Now, let’s jump into Terraform. Create a project directory and initialize it.

1. Create a new directory for your Terraform project:

```bash
mkdir react-app-ec2-terraform
cd react-ec2-terraform
```

2. Create a `main.tf` file:

```bash
touch main.tf
```

3. Open `main.tf` in your code editor (VS Code), copy and paste the configuration below.

⚠️Note: Here’s a Terraform script to deploy a React.js app on AWS using an EC2 instance with Nginx. The setup includes:

✅ Provisioning an EC2 instance <br/>
✅ Installing Nginx <br/>
✅ Deploying a React.js app <br/>
✅ Configuring security groups <br/>
✅ Using user data to automate setup <br/>


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

⚠️Note: Replace `https://github.com/yourusername/your-react-app.git` with your actual GitHub repository URL.

## ➡️ Step 4 - Execute the Terraform Script

1. Initialize Terraform

```bash
terraform init
```

2. Plan and Apply

```bash
terraform apply -auto-approve
```

3. Access Your App via the EC2 public IP

Find the instance in the AWS console and copy the public IP. Visit:

Open it in a browser: `http://your-ec2-public-ip`

You should see your React.js app running! 🚀

## 🗑️ Clean Up Resources

When you’re done, clean up your AWS resources to avoid charges:

```bash
terraform destroy
```


