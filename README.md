# AWS Static Website Hosting using EC2 & S3

## Project Overview
This project demonstrates how to host a simple static website on AWS S3 using EC2 and AWS CLI.

## Prerequisites
- An AWS account
- AWS CLI installed and configured
- EC2 instance running with SSH access
- S3 Bucket created for static website hosting

## Steps to Host the Static Website

### 1. Launch an EC2 Instance
- Log into your AWS Console.
- Launch an EC2 instance (Ubuntu recommended).
- Ensure your instance is in the same region as your S3 bucket.

### 2. Create and Configure an S3 Bucket
- Create an S3 bucket with a globally unique name (e.g., `ttset-s3-static-site-demo`).
- Enable static website hosting in the bucket settings.
- Note the bucket's endpoint URL for later.

### 3. Create an HTML File for Your Static Website
- SSH into your EC2 instance:
  ```bash
  ssh -i your-key.pem ubuntu@your-ec2-ip
  ```
- Create a simple HTML file:
  ```bash
  echo '<h1>Hello from Jimmy!</h1>' > index.html
  aws s3 cp error.html s3://ttset-s3-static-site-demo/
  ```

### 4. Upload the HTML File to S3
- Install the AWS CLI on your EC2 instance if not already installed.
- Upload the HTML file to your S3 bucket:
  ```bash
  aws s3 cp index.html s3://ttset-s3-static-site-demo/
  ```

### 5. Configure S3 Bucket Policy for Public Access
- Create a `bucket-policy.json` file with the following content:
  ```json
  {
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "PublicReadGetObject",
        "Effect": "Allow",
        "Principal": "*",
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::ttset-s3-static-site-demo/*"
      }
    ]
  }
  ```
- Apply the policy:
  ```bash
  aws s3api put-bucket-policy --bucket ttset-s3-static-site-demo --policy file://bucket-policy.json
  ```

### 6. Test Your Static Website
- After uploading and configuring the policy, open the bucket's URL (e.g., `http://ttset-s3-static-site-demo.s3-website-us-east-1.amazonaws.com/`).
- You should see the "Hello from Jimmy!" message.

## Conclusion
You've successfully set up an EC2 instance to upload content to an S3 bucket and configure it for static website hosting.
