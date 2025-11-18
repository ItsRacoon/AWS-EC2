# AWS EC2 Web Server - Complete Setup Guide (Windows)

## 📋 Project Overview

Host a simple website on AWS EC2 using Apache web server. Perfect for college lab exams.

**What you'll do:**
1. Launch a Linux EC2 instance on AWS
2. Install Apache web server
3. Deploy a custom HTML website
4. Access it via public IP

**Time needed:** 30-40 minutes  
**System:** Windows (Command Prompt, PowerShell, or EC2 Instance Connect)

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      Internet                            │
│                    (Your Browser)                        │
└────────────────────────┬────────────────────────────────┘
                         │
                         │ HTTP Request (Port 80)
                         │
            ┌────────────▼──────────────┐
            │   AWS Security Group      │
            │  ┌──────────────────────┐ │
            │  │ Rule 1: SSH (22)     │ │
            │  │ Rule 2: HTTP (80)    │ │
            │  └──────────────────────┘ │
            └────────────┬──────────────┘
                         │
                    ┌────▼─────────────┐
                    │   EC2 Instance   │
                    │  (Amazon Linux)  │
                    │                  │
                    │  ┌────────────┐  │
                    │  │   Apache   │  │
                    │  │ Web Server │  │
                    │  └─────┬──────┘  │
                    │        │         │
                    │  ┌─────▼──────┐  │
                    │  │index.html  │  │
                    │  └────────────┘  │
                    └──────────────────┘
```

---

## 🚀 Step-by-Step Setup

### STEP 1: Launch EC2 Instance (AWS Console)

#### 1.1 Login to AWS Console

1. Open your web browser (Chrome, Edge, Firefox)
2. Go to: **https://aws.amazon.com/console/**
3. Click **"Sign In to the Console"**
4. Enter your AWS account email and password
5. Click **"Sign In"**

#### 1.2 Navigate to EC2 Service

1. You'll see the AWS Management Console homepage
2. At the top, there's a search bar that says "Search for services, features..."
3. Type: **EC2**
4. Click on **"EC2"** (it will say "Virtual Servers in the Cloud")
5. You're now in the EC2 Dashboard

#### 1.3 Launch New Instance

1. Look for the orange button that says **"Launch Instance"**
2. Click **"Launch Instance"**
3. You'll see a page titled "Launch an instance"

#### 1.4 Name Your Instance

1. Under "Name and tags" section
2. In the text box, type: **WebServerProject**
3. This is just a friendly name to identify your server

#### 1.5 Choose Operating System (AMI)

1. Scroll down to "Application and OS Images (Amazon Machine Image)"
2. You'll see several options with logos
3. Look for **"Amazon Linux"** with an orange/red icon
4. Select: **Amazon Linux 2 AMI (HVM) - Kernel 5.10**
5. Make sure it says **"Free tier eligible"** in green

#### 1.6 Choose Instance Type

1. Scroll down to "Instance type"
2. You'll see a dropdown menu
3. Select: **t2.micro**
4. It should show:
   - 1 vCPU
   - 1 GiB Memory
   - **"Free tier eligible"** tag

#### 1.7 Create Key Pair (IMPORTANT for Windows)

1. Scroll down to "Key pair (login)" section
2. Click the **"Create new key pair"** link (blue text)
3. A popup window will appear

**In the popup:**
- **Key pair name:** Type `webserver-key`
- **Key pair type:** Select **RSA**
- **Private key file format:** Select **.pem** (works with Windows PowerShell/CMD)
- Click **"Create key pair"** button

4. A file named `webserver-key.pem` will download to your Downloads folder
5. **IMPORTANT:** Move this file to a safe location (like Documents folder)
6. **DO NOT LOSE THIS FILE** - you can't download it again!

#### 1.8 Configure Network Settings (Security Group)

1. Scroll down to "Network settings"
2. Click **"Edit"** button on the right
3. You'll see "Firewall (security groups)" section

**Configure Security Group:**

Under "Inbound security groups rules" you'll see existing rules. We need 2 rules:

**Rule 1 - SSH (for connecting to server):**
- Type: **SSH**
- Protocol: **TCP**
- Port range: **22**
- Source type: **My IP** (this automatically detects your current IP)
- Description: "SSH access from my computer"

**Rule 2 - HTTP (for website access):**
- Click **"Add security group rule"** button
- Type: **HTTP**
- Protocol: **TCP**
- Port range: **80**
- Source type: **Anywhere** (0.0.0.0/0)
- Description: "Allow web traffic from internet"

#### 1.9 Configure Storage

1. Scroll down to "Configure storage"
2. Default is **8 GiB gp3** (Root volume)
3. Keep the default - this is enough for our project

#### 1.10 Review and Launch

1. On the right side, you'll see a "Summary" panel
2. Review your settings:
   - Name: WebServerProject
   - AMI: Amazon Linux 2
   - Instance type: t2.micro
   - Key pair: webserver-key
   - Security groups: 2 rules (SSH + HTTP)
3. Click the orange **"Launch instance"** button at the bottom right
4. You'll see a success message: "Successfully initiated launch of instance"
5. Click **"View all instances"**

#### 1.11 Wait for Instance to Start

1. You'll see your instance in the list
2. Look at the "Instance state" column
3. It will say "Pending" (yellow) for 1-2 minutes
4. Wait until it says **"Running"** (green dot)
5. Once running, look at the "Public IPv4 address" column
6. **Write down this IP address** - you'll need it! (Example: 54.123.45.67)

---

### STEP 2: Connect to Your Instance (Windows)

You have 3 options to connect. **Option A is easiest for beginners.**

#### **Option A: EC2 Instance Connect (Recommended for Windows - No Setup Needed)**

This method works directly in your browser - no software installation required!

1. In the EC2 Instances page, **select your instance** (click the checkbox)
2. Click the **"Connect"** button at the top
3. You'll see a page with connection options
4. Click the **"EC2 Instance Connect"** tab
5. Keep the default username: **ec2-user**
6. Click the orange **"Connect"** button
7. A new browser tab will open with a black terminal window
8. You'll see a prompt like: `[ec2-user@ip-172-31-xx-xx ~]$`
9. **You're now connected!** Skip to STEP 3.

---

#### **Option B: Using Windows PowerShell (Built-in)**

PowerShell is already installed on Windows 10/11.

**Step 1: Open PowerShell**
1. Press `Windows Key + R` on your keyboard
2. Type: **powershell**
3. Press Enter
4. A blue window will open

**Step 2: Navigate to Your Key File**
```powershell
# Go to Downloads folder (where your .pem file is)
cd Downloads

# Or if you moved it to Documents:
cd Documents

# Check if the file is there
dir webserver-key.pem
```

You should see the file listed with its size and date.

**Step 3: Connect via SSH**
```powershell
# Replace YOUR_PUBLIC_IP with your actual EC2 public IP
ssh -i webserver-key.pem ec2-user@YOUR_PUBLIC_IP
```

**Example:**
```powershell
ssh -i webserver-key.pem ec2-user@54.123.45.67
```

**Step 4: First Time Connection**
- You'll see a message: "The authenticity of host... can't be established"
- Type: **yes**
- Press Enter

**Step 5: You're Connected!**
- You'll see a welcome message with Amazon Linux logo
- Prompt will show: `[ec2-user@ip-172-31-xx-xx ~]$`

---

#### **Option C: Using Windows Command Prompt (CMD)**

**Step 1: Open Command Prompt**
1. Press `Windows Key + R`
2. Type: **cmd**
3. Press Enter
4. A black window will open

**Step 2: Navigate to Your Key File**
```cmd
# Go to Downloads folder
cd %USERPROFILE%\Downloads

# Or if in Documents:
cd %USERPROFILE%\Documents

# Check if file exists
dir webserver-key.pem
```

**Step 3: Connect via SSH**
```cmd
ssh -i webserver-key.pem ec2-user@YOUR_PUBLIC_IP
```

**Example:**
```cmd
ssh -i webserver-key.pem ec2-user@54.123.45.67
```

**Step 4: First Time Connection**
- Type: **yes** when asked
- Press Enter

---

### STEP 3: Install Apache Web Server

Now you're connected to your Linux server. Let's install Apache!

#### 3.1 Update System Packages

Copy this command and paste it in the terminal:

```bash
sudo yum update -y
```

**What this does:**
- `sudo` = run as administrator
- `yum` = package manager for Amazon Linux
- `update` = update all packages
- `-y` = automatically say "yes" to all prompts

**How to paste in terminal:**
- **PowerShell/CMD:** Right-click in the window (it will paste automatically)
- **EC2 Instance Connect:** Right-click and select Paste, or use Ctrl+Shift+V

**Press Enter** and wait 1-2 minutes. You'll see lots of text scrolling. When done, you'll see "Complete!"

#### 3.2 Install Apache

```bash
sudo yum install httpd -y
```

**What this does:**
- `install httpd` = install Apache web server (httpd = HTTP daemon)
- `-y` = automatically confirm installation

**Press Enter** and wait 30 seconds. You'll see "Complete!" at the end.

#### 3.3 Start Apache Service

```bash
sudo systemctl start httpd
```

**What this does:**
- `systemctl` = system control command
- `start httpd` = start the Apache service

**Press Enter**. No output means success!

#### 3.4 Enable Apache to Start on Boot

```bash
sudo systemctl enable httpd
```

**What this does:**
- Makes Apache start automatically when server restarts

You'll see: "Created symlink..." - this is good!

#### 3.5 Check Apache Status

```bash
sudo systemctl status httpd
```

**What to look for:**
- You'll see green text saying **"active (running)"**
- This means Apache is working!

**To exit this view:** Press the **Q** key on your keyboard

---

### STEP 4: Create Your Website

#### 4.1 Navigate to Web Directory

```bash
cd /var/www/html
```

**What this does:**
- `cd` = change directory
- `/var/www/html` = default folder where Apache looks for web files

**Press Enter**. Your prompt will change to show you're in this folder.

#### 4.2 Create HTML File

```bash
sudo nano index.html
```

**What this does:**
- `nano` = simple text editor
- `index.html` = the main webpage file

**Press Enter**. A text editor will open (screen will change to show a blank file).

#### 4.3 Add Website Content

Now you'll see a blank editor. Copy this HTML code:

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
        h1 {
            font-size: 3em;
            margin-bottom: 20px;
        }
        p {
            font-size: 1.3em;
            margin: 10px 0;
        }
        .highlight {
            color: #ffd700;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>🚀 Cloud Computing Project</h1>
        <p>Successfully Hosted on <span class="highlight">AWS EC2</span></p>
        <p>Name: <span class="highlight">YOUR NAME HERE</span></p>
        <p>Roll No: <span class="highlight">YOUR ROLL NUMBER</span></p>
        <p>Course: Cloud Computing Lab</p>
        <p>Server: Apache on Amazon Linux 2</p>
    </div>
</body>
</html>
```

**How to paste in terminal:**
- **PowerShell/CMD:** Right-click in the window
- **EC2 Instance Connect:** Right-click and select Paste, or Ctrl+Shift+V

**IMPORTANT:** Replace "YOUR NAME HERE" and "YOUR ROLL NUMBER" with your actual details!

#### 4.4 Save the File

1. Press **Ctrl + X** (this means "exit")
2. You'll see at the bottom: "Save modified buffer?"
3. Press **Y** (for "yes")
4. You'll see: "File Name to Write: index.html"
5. Press **Enter**
6. You're back to the command prompt

#### 4.5 Verify File Was Created

```bash
ls -la
```

**What this does:**
- `ls` = list files
- `-la` = show all details

You should see `index.html` in the list!

#### 4.6 View File Content (Optional)

```bash
cat index.html
```

This will display your HTML code. Check if your name and roll number are there!

---

### STEP 5: Test Your Website

#### 5.1 Test Locally from Server

```bash
curl localhost
```

**What this does:**
- Tests if Apache is serving the website
- You should see your HTML code displayed

If you see HTML code, Apache is working!

#### 5.2 Test from Your Windows Browser

**Step 1: Get Your Public IP**
1. Go back to AWS EC2 Console in your browser
2. Select your instance
3. Look at "Public IPv4 address" in the details below
4. Copy this IP address (Example: 54.123.45.67)

**Step 2: Open Website in Browser**
1. Open a new browser tab (Chrome, Edge, Firefox)
2. In the address bar, type: `http://YOUR_PUBLIC_IP`
3. **IMPORTANT:** Use `http://` NOT `https://`

**Example:**
```
http://54.123.45.67
```

4. Press Enter
5. **Your website should appear!**

**What you should see:**
- Purple gradient background
- Large heading: "🚀 Cloud Computing Project"
- Your name and roll number
- Professional styling

---

## 📸 Screenshots Needed for Exam

Take these screenshots to show your work:

### Screenshot 1: EC2 Instance Running
- Go to EC2 Dashboard → Instances
- Show your instance with "Running" status (green)
- Public IP visible

### Screenshot 2: Security Group Rules
- Select your instance
- Click "Security" tab
- Click on security group name
- Show both SSH (22) and HTTP (80) rules

### Screenshot 3: Terminal - Apache Status
- In your terminal, run: `sudo systemctl status httpd`
- Show "active (running)" in green
- Take screenshot

### Screenshot 4: Website in Browser
- Full browser window
- Address bar showing `http://YOUR_PUBLIC_IP`
- Your website fully loaded
- Your name and roll number visible

---

## ❓ Common Issues & Fixes

### Issue 1: Website Not Loading in Browser

**Symptom:** Browser shows "This site can't be reached" or "Connection timed out"

**Fix 1: Check Apache is Running**
```bash
sudo systemctl status httpd
```
If not running:
```bash
sudo systemctl start httpd
```

**Fix 2: Check Security Group**
1. Go to EC2 Console
2. Select your instance
3. Click "Security" tab
4. Click security group name
5. Check "Inbound rules"
6. Must have: HTTP, Port 80, Source 0.0.0.0/0

**Fix 3: Use HTTP not HTTPS**
- Wrong: `https://54.123.45.67`
- Correct: `http://54.123.45.67`

---

### Issue 2: Can't Connect via SSH

**Symptom:** "Connection timed out" or "Connection refused"

**Fix 1: Check Instance is Running**
- Go to EC2 Console
- Instance state must be "Running" (green)

**Fix 2: Check Security Group**
- Must have SSH rule (Port 22)
- Source should be "My IP" or "0.0.0.0/0"

**Fix 3: Verify Public IP**
- Copy the correct public IP from EC2 Console
- Public IP changes if you stop/start instance

**Fix 4: Check Key File Location**
```powershell
# In PowerShell, check you're in right folder
dir webserver-key.pem
```

---

### Issue 3: Permission Denied When Creating File

**Symptom:** "Permission denied" when trying to create index.html

**Fix:** Always use `sudo` before commands in /var/www/html
```bash
sudo nano index.html
```

---

### Issue 4: Can't Paste in Terminal

**PowerShell/CMD:**
- Right-click to paste (don't use Ctrl+V)

**EC2 Instance Connect:**
- Right-click and select "Paste"
- Or use Ctrl+Shift+V (not Ctrl+V)

---

## 🎓 Viva Questions (Quick Reference)

### Basic Questions

**Q1: What is EC2?**  
**A:** EC2 (Elastic Compute Cloud) is a service that provides virtual servers in the AWS cloud. You can rent these servers to run applications without buying physical hardware.

**Q2: What is a Security Group?**  
**A:** A security group is a virtual firewall that controls incoming and outgoing traffic to your EC2 instance. It uses rules to allow or block specific types of traffic.

**Q3: Why do we need port 80?**  
**A:** Port 80 is the standard port for HTTP web traffic. When someone types your website URL, their browser connects to port 80 to get the webpage.

**Q4: What is Apache?**  
**A:** Apache is free, open-source web server software. It receives HTTP requests from browsers and sends back web pages (HTML files).

**Q5: What is the difference between Public IP and Private IP?**  
**A:** 
- **Public IP:** Can be accessed from anywhere on the internet (like 54.123.45.67)
- **Private IP:** Only accessible within AWS network (like 172.31.xx.xx)

**Q6: What is IaaS?**  
**A:** IaaS (Infrastructure as a Service) is a cloud model where you rent IT infrastructure (servers, storage, networks) from a provider like AWS. You manage the OS and applications.

**Q7: Why Amazon Linux 2?**  
**A:** It's optimized for AWS, free, secure, and comes with AWS tools pre-installed. It's also lightweight and perfect for web servers.

**Q8: What does t2.micro mean?**  
**A:** It's an instance type with 1 virtual CPU and 1 GB RAM. It's free tier eligible and suitable for small websites and testing.

**Q9: What is an AMI?**  
**A:** AMI (Amazon Machine Image) is a template containing the operating system and software needed to launch an EC2 instance.

**Q10: What happens if you stop vs terminate an instance?**  
**A:**
- **Stop:** Instance is shut down but can be restarted. You keep your data but don't pay for compute.
- **Terminate:** Instance is permanently deleted. All data is lost.

For 30 more detailed viva questions, see `viva-questions.md`

---

## 🧹 Cleanup (After Exam)

### To Stop Instance (Pause - No Charges)
1. Go to EC2 Console
2. Select your instance (checkbox)
3. Click "Instance state" dropdown at top
4. Select "Stop instance"
5. Confirm
6. Wait until status shows "Stopped"

### To Terminate Instance (Delete Permanently)
1. Go to EC2 Console
2. Select your instance
3. Click "Instance state" dropdown
4. Select "Terminate instance"
5. Type "terminate" to confirm
6. Click "Terminate"

**Note:** Terminating deletes everything. You can't recover it!

---

## 📁 Project Files

This project includes:

1. **README.md** (this file) - Complete setup guide with Windows commands
2. **website-template.html** - Professional HTML template you can customize
3. **viva-questions.md** - 40 detailed viva questions with answers

---

## ✅ Quick Command Reference (Copy-Paste)

### Connect to EC2 (PowerShell)
```powershell
cd Downloads
ssh -i webserver-key.pem ec2-user@YOUR_PUBLIC_IP
```

### Install Apache (Linux Terminal)
```bash
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```

### Create Website (Linux Terminal)
```bash
cd /var/www/html
sudo nano index.html
# (paste HTML code, then Ctrl+X, Y, Enter)
```

### Test Website (Linux Terminal)
```bash
curl localhost
```

### Check Logs if Issues (Linux Terminal)
```bash
sudo tail /var/log/httpd/error_log
```

---

## 🎯 Success Checklist

Before calling your examiner, verify:

- [ ] EC2 instance status is "Running" (green)
- [ ] Security group has 2 rules: SSH (22) and HTTP (80)
- [ ] Apache status shows "active (running)"
- [ ] `curl localhost` returns HTML code
- [ ] Website loads in browser at `http://PUBLIC_IP`
- [ ] Your name and roll number are visible on website
- [ ] All 4 screenshots taken
- [ ] Can explain what EC2, Apache, and Security Groups are

---

## 💡 Tips for Exam Day

1. **Practice once before exam** - Do the entire setup to familiarize yourself
2. **Save your public IP** - Write it down or keep EC2 console open
3. **Use EC2 Instance Connect** - Easiest method, no SSH setup needed
4. **Copy-paste commands** - Don't type manually, reduces errors
5. **Take screenshots as you go** - Don't wait until the end
6. **Test in browser before calling examiner** - Make sure everything works
7. **Have viva answers ready** - Review common questions
8. **Don't panic if error occurs** - Check the troubleshooting section

---

## 🆘 Need Help?

If stuck during exam:

1. **Check Apache status first:**
   ```bash
   sudo systemctl status httpd
   ```

2. **Check security group rules** - Most common issue

3. **Verify you're using HTTP not HTTPS**

4. **Check error logs:**
   ```bash
   sudo tail /var/log/httpd/error_log
   ```

5. **Restart Apache:**
   ```bash
   sudo systemctl restart httpd
   ```

---

**Good luck with your exam! 🚀**

You've got this! Follow the steps carefully, and you'll have a working website in 30 minutes.

For detailed viva preparation, open `viva-questions.md`
