# Viva Questions & Answers - EC2 Web Server Project

## Basic Cloud Computing Concepts

### Q1: What is Cloud Computing?
**Answer:** Cloud computing is the delivery of computing services (servers, storage, databases, networking, software) over the internet. Instead of owning physical servers, we rent them from providers like AWS, paying only for what we use.

**Key Benefits:**
- No upfront hardware costs
- Pay-as-you-go pricing
- Scale up or down as needed
- Access from anywhere

---

### Q2: What are the main cloud service models?
**Answer:** 
1. **IaaS (Infrastructure as a Service):** Provides virtual machines, storage, networks. Example: AWS EC2
2. **PaaS (Platform as a Service):** Provides platform for developers. Example: AWS Elastic Beanstalk
3. **SaaS (Software as a Service):** Provides ready-to-use applications. Example: Gmail, Office 365

---

### Q3: What is AWS?
**Answer:** AWS (Amazon Web Services) is the world's largest cloud computing platform, offering over 200 services including compute, storage, databases, and networking. It was launched in 2006 and powers companies like Netflix, Airbnb, and NASA.

---

## EC2 Specific Questions

### Q4: What is EC2?
**Answer:** EC2 (Elastic Compute Cloud) is AWS's virtual server service. It provides resizable compute capacity in the cloud, allowing you to launch virtual machines (called instances) with different configurations of CPU, memory, storage, and networking.

**Why "Elastic"?** You can quickly scale up or down based on demand.

---

### Q5: What is an EC2 Instance?
**Answer:** An EC2 instance is a virtual server running in AWS cloud. It's like having a computer in Amazon's data center that you can access remotely and configure as needed.

---

### Q6: What is an AMI?
**Answer:** AMI (Amazon Machine Image) is a template containing the software configuration needed to launch an instance:
- Operating system (Linux, Windows)
- Application server
- Pre-installed applications

We used **Amazon Linux 2 AMI** which is optimized for AWS and includes AWS tools pre-installed.

---

### Q7: What instance type did you use and why?
**Answer:** We used **t2.micro** because:
- It's free tier eligible (750 hours/month free)
- Sufficient for small websites (1 vCPU, 1 GB RAM)
- Good for learning and testing
- Cost-effective for low-traffic applications

---

### Q8: What is the difference between Stop and Terminate?
**Answer:**

**Stop:**
- Instance is shut down but not deleted
- Can be restarted later
- Data on EBS volume is preserved
- You pay for storage but not compute

**Terminate:**
- Instance is permanently deleted
- Cannot be restarted
- All data is lost (unless backed up)
- No charges after termination

---

## Networking & Security

### Q9: What is a Security Group?
**Answer:** A security group is a virtual firewall that controls traffic to and from your EC2 instance. It works at the instance level and uses rules to allow or deny traffic.

**Key Points:**
- Stateful (if you allow inbound, response is automatically allowed outbound)
- Only allow rules (no deny rules)
- Changes take effect immediately

---

### Q10: What security group rules did you configure?
**Answer:** We configured two inbound rules:

1. **SSH (Port 22):**
   - Protocol: TCP
   - Source: My IP
   - Purpose: Remote access to manage the server

2. **HTTP (Port 80):**
   - Protocol: TCP
   - Source: 0.0.0.0/0 (Anywhere)
   - Purpose: Allow anyone to access the website

---

### Q11: What is the difference between Public IP and Private IP?
**Answer:**

**Public IP:**
- Internet-accessible address
- Visible to the outside world
- Used to access instance from internet
- Changes when instance stops/starts (unless Elastic IP)
- Example: 54.123.45.67

**Private IP:**
- Internal AWS network address
- Not accessible from internet
- Used for communication within AWS
- Remains same throughout instance lifetime
- Example: 172.31.45.67

---

### Q12: What is an Elastic IP?
**Answer:** An Elastic IP is a static public IPv4 address that doesn't change. Normal public IPs change when you stop/start an instance, but Elastic IPs remain the same. Useful for production websites that need a permanent address.

---

### Q13: Why did we allow HTTP from anywhere (0.0.0.0/0)?
**Answer:** Because we want anyone on the internet to access our website. If we restricted it to specific IPs, only those users could view the site. For a public website, we need to allow all sources.

---

### Q14: Why did we restrict SSH to "My IP"?
**Answer:** For security. SSH gives full control of the server, so we only allow access from our own IP address. This prevents unauthorized users from attempting to access or hack the server.

---

## Web Server Questions

### Q15: What is Apache?
**Answer:** Apache HTTP Server is free, open-source web server software that serves web pages to users. When someone types your website URL, Apache receives the request and sends back the HTML page.

**Key Facts:**
- Most popular web server (used by 30%+ of websites)
- Cross-platform (Linux, Windows, Mac)
- Highly customizable
- Developed by Apache Software Foundation

---

### Q16: Why Apache instead of Nginx?
**Answer:**

**Apache Advantages:**
- Easier for beginners
- Better documentation
- More modules available
- Better for dynamic content

**Nginx Advantages:**
- Better performance for static content
- Handles more concurrent connections
- Lower memory usage

For our simple project, Apache is perfect and easier to learn.

---

### Q17: What is the difference between HTTP and HTTPS?
**Answer:**

**HTTP (Port 80):**
- Unencrypted communication
- Data sent in plain text
- Not secure for sensitive information
- What we used in this project

**HTTPS (Port 443):**
- Encrypted communication using SSL/TLS
- Data is secure
- Required for login pages, payments
- Requires SSL certificate

---

### Q18: What does each Apache command do?

**Answer:**

```bash
sudo yum install httpd -y
```
Installs Apache web server package

```bash
sudo systemctl start httpd
```
Starts the Apache service immediately

```bash
sudo systemctl enable httpd
```
Configures Apache to start automatically on boot

```bash
sudo systemctl status httpd
```
Shows current status of Apache (running/stopped)

---

## Linux Commands

### Q19: What does "sudo" mean?
**Answer:** "sudo" stands for "superuser do". It runs commands with administrator/root privileges. We need it to install software, modify system files, and manage services because these operations require elevated permissions.

---

### Q20: What is yum?
**Answer:** yum (Yellowdog Updater Modified) is the package manager for Red Hat-based Linux distributions like Amazon Linux. It's used to install, update, and remove software packages.

Similar tools:
- apt (Ubuntu/Debian)
- dnf (newer Red Hat systems)

---

### Q21: What does the -y flag do?
**Answer:** The `-y` flag automatically answers "yes" to all prompts during installation. Without it, you'd have to manually type "yes" to confirm each step.

---

### Q22: What is systemctl?
**Answer:** systemctl is a command-line tool to manage system services (daemons) in Linux. It's part of systemd, the system and service manager.

Common commands:
- start: Start a service
- stop: Stop a service
- restart: Restart a service
- enable: Auto-start on boot
- status: Check service status

---

### Q23: What is /var/www/html?
**Answer:** It's the default document root directory for Apache web server. Any HTML files placed here are served by Apache. When someone visits your website, Apache looks for index.html in this directory.

---

### Q24: Why do we need sudo to edit files in /var/www/html?
**Answer:** Because /var/www/html is owned by the root user and the apache group. Regular users don't have write permissions to system directories for security reasons. Using sudo temporarily gives us the necessary permissions.

---

## Project Specific

### Q25: Explain the flow when someone accesses your website.
**Answer:**

1. User types `http://YOUR_PUBLIC_IP` in browser
2. Browser sends HTTP request to that IP address
3. Request reaches AWS and goes through Security Group
4. Security Group checks if HTTP (port 80) is allowed
5. Request reaches EC2 instance
6. Apache web server receives the request
7. Apache looks for index.html in /var/www/html
8. Apache sends the HTML file back to user
9. Browser renders the HTML and displays the website

---

### Q26: What would happen if you didn't enable HTTP in security group?
**Answer:** The website wouldn't be accessible. The browser would show "This site can't be reached" or "Connection timed out" because the security group would block all HTTP traffic on port 80. The request would never reach Apache.

---

### Q27: What happens if Apache is not running?
**Answer:** Even if the security group allows HTTP traffic, the browser would show "Connection refused" because there's no service listening on port 80 to handle the request.

---

### Q28: How do you check if Apache is working?
**Answer:** Multiple ways:

1. **Check service status:**
   ```bash
   sudo systemctl status httpd
   ```
   Should show "active (running)" in green

2. **Test locally from server:**
   ```bash
   curl localhost
   ```
   Should return HTML content

3. **Test from browser:**
   Visit `http://PUBLIC_IP`
   Should display the website

---

### Q29: What is the purpose of index.html?
**Answer:** index.html is the default homepage file. When someone visits your website without specifying a file (just the IP or domain), Apache automatically serves index.html. It's the entry point of your website.

---

### Q30: Can you host multiple websites on one EC2 instance?
**Answer:** Yes, using Apache Virtual Hosts. You can configure Apache to serve different websites based on domain names or ports. For example:
- website1.com → /var/www/site1
- website2.com → /var/www/site2

---

## Cost & Billing

### Q31: Is this project free?
**Answer:** Yes, if you stay within AWS Free Tier limits:
- 750 hours/month of t2.micro (enough for 1 instance running 24/7)
- 30 GB of EBS storage
- Valid for 12 months from AWS account creation

After free tier or if you exceed limits, you'll be charged.

---

### Q32: How much does t2.micro cost after free tier?
**Answer:** Approximately $0.0116 per hour or about $8.50 per month if running 24/7. Prices vary by region.

---

### Q33: How can you reduce costs?
**Answer:**
- Stop instances when not in use (no compute charges)
- Terminate instances you don't need
- Use reserved instances for long-term (cheaper)
- Monitor usage with AWS Cost Explorer
- Set up billing alerts

---

## Troubleshooting

### Q34: Website not loading - how do you troubleshoot?
**Answer:** Follow this checklist:

1. **Check instance status:** Must be "Running"
2. **Check Apache status:** `sudo systemctl status httpd`
3. **Check security group:** HTTP rule must allow port 80 from 0.0.0.0/0
4. **Verify public IP:** Copy correct IP from console
5. **Use HTTP not HTTPS:** `http://` not `https://`
6. **Check firewall:** `sudo systemctl status firewalld`
7. **Check logs:** `sudo tail /var/log/httpd/error_log`

---

### Q35: How do you view Apache error logs?
**Answer:**
```bash
sudo tail -f /var/log/httpd/error_log
```

The `-f` flag follows the log in real-time, showing new errors as they occur.

---

## Advanced Questions

### Q36: What is the difference between vertical and horizontal scaling?
**Answer:**

**Vertical Scaling (Scale Up):**
- Increase instance size (t2.micro → t2.large)
- More CPU, RAM, storage
- Limited by maximum instance size
- Requires downtime

**Horizontal Scaling (Scale Out):**
- Add more instances
- Use load balancer to distribute traffic
- Unlimited scaling potential
- No downtime

---

### Q37: What is a Load Balancer?
**Answer:** A load balancer distributes incoming traffic across multiple EC2 instances. If one instance fails, traffic is routed to healthy instances. This improves availability and handles more users.

---

### Q38: What is Auto Scaling?
**Answer:** Auto Scaling automatically adjusts the number of EC2 instances based on demand. When traffic increases, it launches more instances. When traffic decreases, it terminates extra instances. This optimizes cost and performance.

---

### Q39: How would you make this project production-ready?
**Answer:**

1. **Use HTTPS:** Get SSL certificate (AWS Certificate Manager)
2. **Use domain name:** Register domain and use Route 53
3. **Use Elastic IP:** Permanent IP address
4. **Add Load Balancer:** Distribute traffic across multiple instances
5. **Enable Auto Scaling:** Handle traffic spikes
6. **Set up monitoring:** CloudWatch for alerts
7. **Regular backups:** Snapshot EBS volumes
8. **Use RDS:** For database instead of local storage
9. **CDN:** CloudFront for faster content delivery
10. **Security:** Regular updates, strong passwords, key rotation

---

### Q40: What other AWS services could enhance this project?
**Answer:**

- **Route 53:** DNS service for custom domain
- **CloudFront:** CDN for faster global access
- **S3:** Store static files (images, CSS, JS)
- **RDS:** Managed database service
- **CloudWatch:** Monitoring and logging
- **Certificate Manager:** Free SSL certificates
- **Elastic Load Balancer:** Distribute traffic
- **Auto Scaling:** Automatic capacity adjustment
- **IAM:** Manage access and permissions

---

**Good luck with your viva! 🎓**
