Deploying Flask Application to AWS Cloud with ECS
This guide provides a step-by-step walkthrough for deploying a Flask web application from a local repository to the AWS Cloud using ECS (Elastic Container Service). It covers the setup of environment variables, configuration of MongoDB, deployment of the Flask app, and securing the application using Nginx.

Table of Contents
AWS Security Group Setup
MongoDB Installation and Setup
MongoDB Installation
MongoDB Configuration
MongoDB Global Access Configuration
Flask Application Deployment
Setting Up the Python Environment
Running the Flask Application
Gunicorn and Systemd Setup
Nginx Setup
Final Testing
1. AWS Security Group Setup
Configure the security group for your EC2 instance to allow the necessary traffic:

Security Group Rules:

SSH: Protocol TCP, Port 22, Source 0.0.0.0/0 and ::/0
HTTP: Protocol TCP, Port 80, Source 0.0.0.0/0 and ::/0
HTTPS: Protocol TCP, Port 443, Source 0.0.0.0/0 and ::/0
Custom TCP: Protocol TCP, Port 8080, Source 0.0.0.0/0 and ::/0
All TCP: Protocol TCP, Port Range 1-12000, Source 0.0.0.0/0 and ::/0
2. MongoDB Installation and Setup
Connect to your EC2 Instance
Use SSH to connect to your EC2 instance. Use .pem key for Ubuntu or .ppk for Putty:

Example:
ssh -i your-key.pem ubuntu@your-ec2-ip

Switch to root user with the following command:
sudo su

MongoDB Installation
Install required packages: gnupg and curl.
Import the MongoDB public key using curl and gpg commands.
Add MongoDB repository by modifying the sources list for Ubuntu.
Update the package list using sudo apt-get update.
Install MongoDB by running sudo apt-get install -y mongodb-org.
MongoDB Configuration
To hold MongoDB packages at their current version, use the following dpkg commands:

Hold mongodb-org, mongodb-org-database, mongodb-org-server, mongodb-mongosh, mongodb-org-mongos, and mongodb-org-tools.
Manage the MongoDB service:

Start MongoDB: sudo systemctl start mongod
Reload the system daemon: sudo systemctl daemon-reload
Check MongoDB status: sudo systemctl status mongod
Enable MongoDB to start on boot: sudo systemctl enable mongod
Stop and restart MongoDB as needed.
Access the MongoDB shell by typing mongosh. To exit the shell, type exit().

MongoDB Global Access Configuration
Open the MongoDB configuration file located at /etc/mongod.conf.
Modify the bindIp field to 0.0.0.0 to allow remote connections.
Restart MongoDB using: sudo service mongod restart.
3. Flask Application Deployment
Setting Up the Python Environment
Update the system package list using sudo apt-get update.

Install python3-venv and python3-pip packages for setting up a virtual environment.

Clone your Git repository using a command like:
git clone 'https://github.com/your-username/flask-deployment.git'

Change to the repository directory:
cd flask-deployment

Create a virtual environment:
python3 -m venv venv

Activate the virtual environment with the command:
source venv/bin/activate

Install dependencies listed in requirements.txt using:
pip install -r requirements.txt

Running the Flask Application
Activate the virtual environment if not already active:

source venv/bin/activate
To run the Flask application, use the command:
python3 application.py

Ensure the MongoDB connection string is properly configured in application.py.

4. Gunicorn and Systemd Setup
Install Gunicorn in the virtual environment using:
pip install gunicorn

Create a Systemd service file for Gunicorn by editing:
sudo nano /etc/systemd/system/flaskapp.service

Add the following content to the service file:
```bash
  [Unit]
Description=Gunicorn instance for a simple flask app
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/flask-deployment
ExecStart=/home/ubuntu/flask-deployment/venv/bin/gunicorn -b localhost:8000 application:application
Restart=always

[Install]
WantedBy=multi-user.target

Reload the Systemd daemon with the command:
sudo systemctl daemon-reload

Start and enable the Gunicorn service using:
sudo systemctl start flaskapp
sudo systemctl enable flaskapp

To test the setup, run:
curl localhost:8000

Nginx Setup
Install Nginx using the command:
sudo apt-get install nginx

Start and enable Nginx:
sudo systemctl start nginx
sudo systemctl enable nginx

Configure Nginx to proxy requests to Gunicorn by editing the Nginx configuration file:
sudo nano /etc/nginx/sites-available/default

Add the following configuration
upstream flaskapp {
    server 127.0.0.1:8000;
}

server {
    listen 80;

    location / {
        proxy_pass http://flaskapp;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

Restart Nginx to apply changes using:
sudo systemctl restart nginx
