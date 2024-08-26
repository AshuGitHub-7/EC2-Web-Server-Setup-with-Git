### ec2-webserver-git-setup

"Comprehensive guide and code for setting up a web server on an AWS EC2 instance using Git. Includes steps for launching an instance, configuring a web server, setting up Git for automatic deployment, and securing the site with SSL/TLS."

Here's a comprehensive guide for setting up a web server project on an EC2 instance using Git, with the necessary code and step-by-step instructions.

### 1. *Launch an EC2 Instance*

1. *Login to AWS Console*:
   - Go to the EC2 dashboard and launch a new instance.

2. *Select AMI*:
   - Choose Amazon Linux 2023 or Ubuntu.

3. *Instance Type*:
   - Choose t2.micro (free tier eligible).

4. *Configure Instance*:
   - Keep default settings or adjust as needed.

5. *Add Storage*:
   - Use the default 8 GB or increase as per your needs.

6. *Configure Security Group*:
   - Allow SSH (port 22) and HTTP (port 80).
   - Optionally, allow HTTPS (port 443) for SSL later.

7. *Key Pair*:
   - Create or choose an existing key pair for SSH access.

8. *Launch the Instance*:
   - Start the instance and note the public IP.

### 2. *Connect to the EC2 Instance*

1. *SSH into Instance*:
   - Use the following command on your local machine:
     bash
     ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip
     
   - For Ubuntu:
     bash
     ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
     

### 3. *Install Git and Web Server*

1. *Update the Instance*:
   - Run the following commands to update the package lists:
     ```
     sudo yum update -y    # Amazon Linux
     sudo apt-get update -y # Ubuntu
     ```

2. *Install Git*:
   - Install Git by running:
     ```
     sudo yum install git -y    # Amazon Linux
     sudo apt-get install git -y # Ubuntu
     ```

3. *Install Apache or Nginx*:
   - For Apache:
     ```
     sudo yum install httpd -y    # Amazon Linux
     sudo apt-get install apache2 -y # Ubuntu
     ```
   - For Nginx:
     ```
     sudo yum install nginx -y    # Amazon Linux
     sudo apt-get install nginx -y # Ubuntu
     ```

4. *Start the Web Server*:
   - Start the web server:
     ```
     sudo systemctl start httpd    # Apache
     sudo systemctl start apache2  # Apache on Ubuntu
     sudo systemctl start nginx    # Nginx
     ```

5. *Enable Web Server on Boot*:
   - Ensure the web server starts on reboot:
     ```
     sudo systemctl enable httpd    # Apache
     sudo systemctl enable apache2  # Apache on Ubuntu
     sudo systemctl enable nginx    # Nginx
     ```

### 4. *Set Up a Git Repository for the Web Server*

1. *Navigate to Web Directory*:
   - Apache:
     ```
     cd /var/www/html
     ```
   - Nginx:
     ```
     cd /usr/share/nginx/html
     ```

2. *Initialize a Git Repository*:
   - Run:
     ```
     sudo git init
     ```

3. *Configure Git User*:
   - Set up Git user information:
     ```
     git config --global user.name "Your Name"
     git config --global user.email "your.email@example.com"
     ```

### 5. *Add and Commit Web Content*

1. *Create a Basic HTML File*:
   - Apache:
     ```
     echo "<h1>Hello, World!</h1>" | sudo tee index.html
     ```
   - Nginx:
     ```
     echo "<h1>Hello, World!</h1>" | sudo tee index.html
     ```

2. *Stage and Commit the File*:
   - Run:
     ```
     sudo git add index.html
     sudo git commit -m "Initial commit with index.html"
     ```

### 6. *Set Up a Remote Git Repository (Optional)*

1. *Create a Repository on GitHub*:
   - Log in to GitHub and create a new repository.

2. *Link to Remote Repository*:
   - Connect your local repository to GitHub:
     ```
     sudo git remote add origin https://github.com/username/repository.git
     sudo git branch -M main
     sudo git push -u origin main
     ```

### 7. *Set Up a Post-Receive Hook for Automatic Deployment*

1. *Create a Post-Receive Hook*:
   - Navigate to the Git hooks directory:
     ```
     cd /var/www/html/.git/hooks    # Apache
     cd /usr/share/nginx/html/.git/hooks # Nginx
     ```

2. *Create the post-receive Hook*:
   - Create the hook script:
     ```
     sudo nano post-receive
     ```
   - Add the following content to the script:
     ```
     #!/bin/bash
     GIT_WORK_TREE=/var/www/html git checkout -f   # Apache
     GIT_WORK_TREE=/usr/share/nginx/html git checkout -f # Nginx
     ```
   - Save and exit.

3. *Make the Hook Executable*:
   - Run:
     ```
     sudo chmod +x post-receive
     ```

### 8. *Push Changes from Your Local Machine*

1. *Clone the Repository Locally*:
   - Clone the GitHub repository to your local machine:
     ```
     git clone https://github.com/username/repository.git
     cd repository
     ```

2. *Make Changes to the Website*:
   - Modify index.html or other files locally.

3. *Commit and Push Changes*:
   - Stage, commit, and push the changes:
     ```
     git add .
     git commit -m "Updated website content"
     git push origin main
     ```

4. *Deploy Changes Automatically*:
   - The changes will be automatically deployed to your EC2 instance thanks to the post-receive hook.

### 9. *Access Your Website*

1. *View Your Website*:
   - Open a web browser and navigate to http://your-ec2-public-ip to view your deployed website.

### 10. *Set Up HTTPS with SSL/TLS (Optional)*

1. *Install Certbot*:
   - Install Certbot for Apache or Nginx:
     ```
     sudo yum install certbot python3-certbot-apache -y   # Apache
     sudo yum install certbot python3-certbot-nginx -y    # Nginx
     ```

2. *Run Certbot*:
   - Obtain and install the SSL certificate:
     ```
     sudo certbot --apache    # Apache
     sudo certbot --nginx     # Nginx
     ```

3. *Follow the Prompts*:
   - Certbot will guide you through securing your website with HTTPS.

This step-by-step guide should help you set up a web server project on an EC2 instance using Git, complete with automatic deployment and optional HTTPS setup.
