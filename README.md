# Nginx Load Balancer for Static Websites

This repository demonstrates setting up an Nginx load balancer to distribute traffic between two static websites deployed on separate servers.

---

# Table of Contents

- [Nginx Load Balancer for Static Websites](#nginx-load-balancer-for-static-websites)
  - [Setup Overview](#setup-overview)
    - [Requirements](#requirements)
    - [Steps](#steps)
      - [1. Deploy Servers](#1-deploy-servers)
        - [Ensuring Servers are Set Up and Accessible via SSH](#ensuring-servers-are-set-up-and-accessible-via-ssh)   
      - [2. Set Up Static Websites (Server A & Server B)](#2-set-up-static-websites-server-a--server-b)
      - [3. Configure Load Balancer (Server C)](#3-configure-load-balancer-server-c)
      - [4. DNS Configuration](#4-dns-configuration)
        - [Adding Load Balancer IP to DNS A Record](#adding-load-balancer-ip-to-dns-a-record) 
      - [5. Testing](#5-testing)
      - [6. Explore Load Balancing Options](#6-explore-load-balancing-options)
      - [7. Understanding L7 Load Balancing](#7-understanding-l7-load-balancing)
  - [Resources](#resources)
  - [License](#license)

## Setup Overview

### Requirements
- Three servers: Server A, Server B, Server C
- Nginx installed on all three servers

---

### Steps

#### 1. Deploy Servers
Ensure all servers are set up and accessible via SSH.

#### 2. Set Up Static Websites (Server A & Server B)

##### Server A & Server B
- Install Nginx:
  
  ```bash
  sudo apt update
  sudo apt install nginx
  ```

- Configure websites:
  
  - Server A:
  
    ``` bash
    sudo mkdir -p /var/www/site1
    echo "Server A" | sudo tee /var/www/site1/index.html
    ```

   - Server B:

     ```bash
     sudo mkdir -p /var/www/site2
     echo "Server B with a change" | sudo tee /var/www/site2/index.html
     ```

#### 3. Configure Load Balancer (Server C)
##### Server C

- Install Nginx:

  ```bash
  sudo apt update
  sudo apt install nginx
  ```

- Configure Nginx as a load balancer:

  - Edit nginx.conf:

    ```
    sudo nano /etc/nginx/nginx.conf
    ```

 - Add the following upstream block and server block within the http block:
  
   ```nginx
   upstream my_sites {
       server <ServerA_IP>;
       server <ServerB_IP>;
   }

   server {
       listen 80;
       server_name example.com;

       location / {
           proxy_pass http://my_sites;
       }
   }
   ```
   Replace `<ServerA_IP>` and `<ServerB_IP>` with the actual IP addresses of Server A and Server B.

---

#### 4. DNS Configuration
Add Server C's IP address (load balancer) to the DNS A record of your domain.

---

#### 5. Testing
Access the website (example.com). With each reload, you should see different content from Server A or Server B, indicating load balancing is functional.

---

#### 6. Explore Load Balancing Options
Experiment with different Nginx load balancing algorithms and options within the nginx.conf file.

---

#### 7. Understanding L7 Load Balancing
Learn about Layer 7 (L7) load balancing for more sophisticated routing decisions based on application-level data.

---

### Ensuring Servers are Set Up and Accessible via SSH

To ensure servers are properly set up and accessible via SSH, follow these steps:

#### Step 1: Obtain Server Credentials

- Gather necessary credentials:
  
  - IP addresses of each server (Server A, Server B, Server C).
  - Username and password or SSH key for accessing each server.

#### Step 2: Connect to Servers

- Open a terminal or SSH client on your local machine.

#### Step 3: Connect to Server A

- Use the following command to connect via SSH to Server A:

  ```bash
  ssh username@ServerA_IP
  ```
  Replace `username` with your server username and `ServerA_IP` with the IP address of Server A.

- If prompted, enter the password or SSH key passphrase to authenticate.

#### Step 4: Verify Connection

Once connected to Server A, ensure access for executing commands. Perform basic checks like checking server status and available disk space to ensure proper functionality.

#### Step 5: Repeat for Server B and Server C

Follow the same process as in Step 3 for Server B and Server C, replacing the IP addresses accordingly. Verify connections to each server and perform basic checks to ensure accessibility and functionality.

#### Step 6: SSH Key Authentication (Optional)

For enhanced security, consider implementing SSH key-based authentication:
- Generate SSH keys locally using `ssh-keygen`.
- Copy the public key generated (`id_rsa.pub`) to each server's `~/.ssh/authorized_keys` file.

#### Step 7: Update Firewall Rules (if needed)

If servers have firewalls enabled, ensure that SSH ports (usually port 22) are open to allow SSH connections. Update firewall rules using the appropriate commands or firewall management tools.

#### Step 8: Document Server Details

Maintain a record of essential server information for future reference, including:
- Server IP addresses
- SSH usernames
- Any other necessary configuration details

By following these steps, you'll ensure that all servers are properly set up, accessible via SSH, and ready for further configuration and setup.

---

### Adding Load Balancer IP to DNS A Record

To direct traffic to the load balancer (Server C) using your domain, follow these steps:

#### Step 1: Access DNS Management Console

- Log in to your domain registrar's website or access your DNS management console.

#### Step 2: Locate DNS Settings

- Navigate to the section where you manage DNS settings for your domain.

#### Step 3: Add A Record

- Find the option to add a new DNS record (usually labeled as "Add Record," "Add DNS," etc.).

#### Step 4: Configure A Record

- Select the record type as "A" (Address).

#### Step 5: Enter Details

- In the "Name" or "Host" field, add the desired subdomain or leave it blank to use the root domain.
- In the "Value," "IPv4 Address," or similar field, enter the IP address of Server C (the load balancer).
- Set the TTL (Time to Live) based on your preference or leave it as default.

#### Step 6: Save Changes

- Save the new A record configuration.

#### Step 7: Verify Changes

- Wait for DNS propagation (it may take some time depending on your DNS provider).
- Verify the changes by pinging the domain or accessing the domain in a web browser to ensure it points to the load balancer's IP address (Server C).

This will direct incoming traffic for your domain to the load balancer, allowing it to distribute the traffic between Server A and Server B.

---

## Resources
- [Nginx Documentation](https://nginx.org/en/docs/)
- [Understanding Nginx Load Balancing](https://www.nginx.com/resources/glossary/load-balancing/)
- [DigitalOcean: How To Set Up Nginx Load Balancing](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-load-balancing)

---

## License
This project is licensed under the [MIT License](https://github.com/iftekharmickey/Nginx-Load-Balancer-for-Static-Websites/blob/main/LICENSE)
