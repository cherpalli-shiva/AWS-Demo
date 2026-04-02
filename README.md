# AWS-Demo — Static Website Deployment on AWS EC2
![screenshot1_website_live](https://github.com/user-attachments/assets/286a387a-be63-457b-9862-02fdb5ce1b70)

A fully responsive static website deployed on AWS EC2 (Ubuntu) using Apache2 and optionally Node.js + Express. This project demonstrates end-to-end DevOps skills — from local Git setup to live cloud deployment.

URL: http://18.220.213.34 (Apache2)
Node.js URL: http://18.220.213.34:3000 (Express server)

**📁 Project Structure**
AWS-Demo/
├── index.html              # Main landing page
├── error/
│   └── index.html          # Custom error page
├── images/
│   ├── bg.jpg
│   ├── overlay.png
│   └── pic01-03.jpg
├── assets/
│   ├── css/                # Stylesheets (main, font-awesome, noscript)
│   ├── js/                 # JavaScript (jQuery, skel, main, util)
│   ├── sass/               # SASS source files
│   └── fonts/              # FontAwesome icon fonts
├── server.js               # Node.js Express server (optional)
├── .gitignore
└── README.md

**🛠️ Tech Stack**
Layer                       Technology
Cloud Provider              AWS EC2 (Ubuntu 24.04 LTS)
Web Server                  Apache2 / Node.js Express
Frontend                    HTML5, CSS3, JavaScript
CSS Framework               SASS / FontAwesome
Version Control             Git + GitHub
Process Manager             PM2 (Node.js)

**⚙️ Deployment Guide**
**Prerequisites**
AWS Account with EC2 access
EC2 Instance: Ubuntu 24.04 LTS (t2.micro)
Security Group: Ports 22 (SSH), 80 (HTTP), 3000 (Node.js) open
Git installed locally


**Step 1 — Local Git Setup**
# Initialize repository
git init
git add .
git commit -m "First commit - Entire project"

# Connect to GitHub
git remote add origin https://github.com/cherpalli-shiva/AWS-Demo.git
git branch -M main

# Pull remote README then push
git pull origin main --allow-unrelated-histories --no-rebase
git push -u origin main


**Step 2 — EC2 Server Setup (Apache2)**
# Update packages and install Apache
sudo apt update
sudo apt install apache2 -y

# Start and enable Apache on boot
sudo systemctl start apache2
sudo systemctl enable apache2

# Deploy project files
sudo cp -r ~/AWS-Demo/* /var/www/html/

# Verify deployment
sudo systemctl status apache2
Access: http://<your-ec2-public-ip>

**Step 3 — Optional: Node.js Deployment**
# Install Node.js and npm
sudo apt install nodejs npm -y

# Navigate to project
cd ~/AWS-Demo
npm init -y
npm install express

# Create server.js
cat > server.js << 'EOF'
const express = require('express');
const app = express();
app.use(express.static(__dirname));
app.listen(3000, () => console.log('Server running on port 3000'));
EOF

# Install PM2 and run
sudo npm install -g pm2
pm2 start server.js --name "aws-demo"
pm2 startup && pm2 save
Access: http://<your-ec2-public-ip>:3000

**🔧 Troubleshooting**
**Issue                                                              Fix**
httpd not found                                                    Use apache2 on Ubuntu (not httpd)  
Permission denied on /var/www/html                                 Use sudo cp
git push rejected                                                  Run git pull origin main --allow-unrelated-histories first  
Divergent branches error                                           Use --no-rebase flag with pull
Port 3000 not accessible                                           Add inbound rule in EC2 Security Group

