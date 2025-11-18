# AWS EC2 Web Server - Complete Setup Guide

## 📋 Project Overview

Host a simple website on AWS EC2 using Apache web server. Perfect for college lab exams.

**What you'll do:**
1. Launch a Linux EC2 instance on AWS
2. Install Apache web server
3. Deploy a custom HTML website
4. Access it via public IP

**Time needed:** 30-40 minutes

---

## 🏗️ Architecture

```
Internet → Security Group (Port 80) → EC2 Instance → Apache → index.html
```

---

## 🚀 Step-by-Step Setup

### STEP 1: Launch EC2 Instance

1. **Login to AWS Console**
   - Go to: https://aws.amazon.com/console/
   - Sign in with your account

2. **Go to EC2**
   - Search "EC2" in top search bar
   - Click EC2 service

3. **Launch Instance**
   - Click orange "Launch Instance" button
   - Name: `WebServerProject`

4. **Choose Settings**
   - **AMI:** Amazon Linux 2 AMI (HVM)
   - **Instance Type:** t2.micro (Free tier)
   - **Key Pair:** Create new → Name: `webserver-key` → Download .pem file

5. **Security Group (Important!)**
   - Add Rule 1: SSH, Port 22, Source: My IP
   - Add Rule 2: HTTP, Port 80, Source: Anywhere (0.0.0.0/0)

6. **Launch**
   - Click "Launch Instance"
   - Wait until Status = "Running" (green)
   - Note your Public IP address

---

### STEP 2: Connect to Instance

**Option A: EC2 Instance Connect (Easiest)**
1. Select your instance
2. Click "Connect" button
3. Choose "EC2 Instance Connect"
4. Click "Connect" → Browser terminal opens

**Option B: SSH (Windows/Mac/Linux)**
```bash
# Windows/Mac/Linux
ssh -i webserver-key.pem ec2-user@YOUR_PUBLIC_IP

# If permission error on Mac/Linux:
chmod 400 webserver-key.pem
```

---

### STEP 3: Install Apache

Copy and paste these commands one by one:

```bash
# Update system
sudo yum update -y

# Install Apache
sudo yum install httpd -y

# Start Apache
sudo systemctl start httpd

# Enable Apache to start on boot
sudo systemctl enable httpd

# Check status (should show "active")
sudo systemctl status httpd
```

Press `q` to exit status view.

---

### STEP 4: Create Your Website

```bash
# Go to web directory
cd /var/www/html

# Create HTML file
sudo nano index.html
```

**Copy this HTML code** (or use website-template.html from this project):

```html
<!DOCTYPE html>
<html>
<head>
    <title>Cloud Computing Project</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .container {
            text-align: center;
            background: rgba(255, 255, 255, 0.1);
            padding: 50px;
            border-radius: 15px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }
        h1 { font-size: 3em; margin-bottom: 20px; }
        p { font-size: 1.3em; margin: 10px 0; }
        .highlight { color: #ffd700; font-weight: bold; }
    </style>
</head>
<body>
    <div class="container">
        <h1>🚀 Cloud Computing Project</h1>
        <p>Successfully Hosted on <span class="highlight">AWS EC2</span></p>
        <p>Name: <span class="highlight">YOUR NAME</span></p>
        <p>Roll No: <span class="highlight">YOUR ROLL NUMBER</span></p>
        <p>Course: Cloud Computing Lab</p>
        <p>Server: Apache on Amazon Linux 2</p>
    </div>
</body>
</html>
```

**Save the file:**
- Press `Ctrl + X`
- Press `Y`
- Press `Enter`

---

### STEP 5: Test Your Website

```bash
# Test locally
curl localhost
```

**Open in Browser:**
1. Copy your EC2 Public IP
2. Open browser
3. Type: `http://YOUR_PUBLIC_IP` (use http NOT https)
4. Your website should appear!

---

## 📸 Screenshots Needed for Exam

1. EC2 instance showing "Running" status
2. Security group rules (SSH + HTTP)
3. Terminal showing Apache status "active"
4. Browser showing your website with URL visible

---

## ❓ Common Issues & Fixes

### Website not loading?
```bash
# Check Apache is running
sudo systemctl status httpd

# If not running, start it
sudo systemctl start httpd

# Check security group has HTTP (port 80) rule with 0.0.0.0/0
```

### Can't connect via SSH?
- Check security group has SSH rule
- Verify you're using correct public IP
- Ensure instance is "Running"

### Permission denied?
- Always use `sudo` for commands in /var/www/html

---

## 🎓 Viva Questions (Quick Reference)

**Q: What is EC2?**  
A: Elastic Compute Cloud - virtual servers in AWS cloud

**Q: What is a Security Group?**  
A: Virtual firewall controlling inbound/outbound traffic

**Q: Why port 80?**  
A: HTTP uses port 80 for web traffic

**Q: What is Apache?**  
A: Open-source web server software that serves web pages

**Q: Public vs Private IP?**  
A: Public IP is internet-accessible, Private IP is internal AWS network only

**Q: What is IaaS?**  
A: Infrastructure as a Service - rent virtual infrastructure from cloud provider

For detailed viva questions, see `viva-questions.md`

---

## 🧹 Cleanup (After Exam)

**Stop Instance (to save costs):**
1. Select instance
2. Instance State → Stop

**Terminate Instance (delete completely):**
1. Select instance
2. Instance State → Terminate
3. Confirm

---

## 📁 Project Files

- `SETUP-GUIDE.md` - This file (complete setup instructions)
- `website-template.html` - Professional HTML template
- `viva-questions.md` - 40 detailed viva Q&A

---

## ✅ Quick Command Reference

```bash
# Connect
ssh -i webserver-key.pem ec2-user@YOUR_PUBLIC_IP

# Install Apache
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd

# Create website
cd /var/www/html
sudo nano index.html
# (paste HTML, save with Ctrl+X, Y, Enter)

# Check status
sudo systemctl status httpd
curl localhost

# View logs if issues
sudo tail /var/log/httpd/error_log
```

---

## 🎯 Success Checklist

- [ ] EC2 instance running
- [ ] Security group has HTTP (80) and SSH (22) rules
- [ ] Apache status shows "active (running)"
- [ ] Website loads in browser at http://PUBLIC_IP
- [ ] Name and roll number updated in HTML
- [ ] Screenshots taken
- [ ] Can explain what EC2, Apache, and Security Groups are

---

**Good luck with your exam! 🚀**

For detailed viva preparation, check `viva-questions.md`
