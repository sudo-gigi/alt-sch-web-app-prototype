# Alt-Sch-Web-App-Prototype

## Project Overview  
This project demonstrates how to deploy a simple HTML-based landing page on an AWS EC2 instance with Nginx as the web server and secure it with HTTPS using Certbot. A domain from No-IP was used to link the Elastic IP to a custom subdomain.       

## Features  
- A landing page with the project-overview, bio, tech journey, and goals.  
- Hosted on an Nginx web server.
- Custom subdomain linked through [No-IP](https://www.noip.com).   
- Secured with HTTPS using Certbot.  

## Deployment Instructions  
Follow these steps to deploy the web application:

### 1. **Provisioning the Server**

#### Platform: AWS
- Log into the AWS Management Console.
- Launch a `t2.micro` EC2 instance using Ubuntu 22.04 LTS as the AMI.

#### Key Pair:
- Create and download a `.pem` key pair for SSH access.

#### Security Group:
- Configure the security group to allow:
  - SSH (port 22)
  - HTTP (port 80)
  - HTTPS (port 443)

#### Elastic IP:
- Allocate an Elastic IP and associate it with the EC2 instance.

### 2. **Setting Up the Web Server**

#### SSH Access:
Using the `.pem` key you can ssh into the instance, however, i connected to mine using EC2 Instance Connect.
- Connect to the EC2 instance using EC2 Instance Connect.

#### Update and Install Nginx:
- Update the package manager:
```bash
     sudo apt update
     sudo apt upgrade -y
```
- Install Nginx:
```bash
     sudo apt update
     sudo apt upgrade -y
```
- Start and enable Nginx to run at startup:
```bash
     sudo systemctl start nginx
     sudo systemctl enable nginx
```
- Ensure that Nginx is running properly:
```bash
     sudo systemctl status nginx
```
You should see a message indicating that Nginx is active and running.
Once Nginx is up and running, you can access your server’s elastic IP in a browser. You should see the default Nginx welcome page.

### 3. **Deploy the HTML page**

#### Navigate to Nginx's Web Directory

Now there are diffrent ways to get your HTML page on the server, however, since we are deploying a simple landing page, you can copy and paste your HTML content into a new file using a text editor like nano.

Firstly, Nginx serves content from the `/var/www/html` directory. We'll place your HTML page in this directory.

```bash
cd /var/www/html
```
- Paste your HTML file
While in `/var/www/html` paste your HTML content and then save (Ctrl+O) and exit (Ctrl+X).
```bash
sudo nano /var/www/html/index.html
```
- Set permissions: Ensure that the index.html file has the correct permissions so that Nginx can serve it:
```bash
     sudo chown www-data:www-data /var/www/html/index.html
     sudo chmod 644 /var/www/html/index.html
```
- Check Your Website
In your browser, go to the public IP address of your EC2 instance (e.g., http://<your-elastic-ip>) to confirm that your HTML page is being served.

### 4. Acquiring a Domain from No-IP

#### Registration:
- Register an account on [No-IP](https://www.noip.com).
- Create a free subdomain (e.g., `cloudupgigi.ddns.net`).

#### DNS Settings:
- Update the A record to point to the Elastic IP of the EC2 instance.

#### Verify DNS Propagation:
- Use the `dig` command to confirm:
```bash
dig your-domain-name NS
```

### 5. Enabling HTTPS with Certbot

#### Install Certbot:
- Adde Certbot and Nginx plugins:
```bash
sudo apt install certbot python3-certbot-nginx -y
```
### Run Certbot to Obtain the Certificate:

Certbot will automatically configure SSL for your domain with Nginx.
```bash
sudo certbot --nginx -d <your-domain>
```
When prompted, follow the instructions to:
- Agree to the terms of service.
- Enter your email address.
- Allow Certbot to configure HTTPS automatically.

### Verify the Certificate Installation:

Certbot will update your Nginx configuration to include the SSL certificate.

Restart Nginx to apply changes:
```bash
sudo systemctl restart nginx
```
```bash
sudo certbot certificates
```
This will show details of the installed certificate, including the domain name, expiration date, and file locations.

### Test HTTPS Access:

- Open your browser and visit `https://yourdomain.com`.
- Ensure that the page loads with a padlock icon, indicating the connection is secure.

That's all folks! 🙂🎉

## Public Access  
My landing page is publicly accessible via the custom subdomain:  
`https://cloudupgigi.ddns.net` 

## Screenshots  
Below is a screenshot of the deployed landing page viewed in a browser:  

![Landing Page Screenshot](screenshots/landing-page.png)  

