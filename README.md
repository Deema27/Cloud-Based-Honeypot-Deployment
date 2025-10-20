# Cloud-Based-Honeypot-Deployment
## Overview
This project demonstrates the deployment and operation of the T-Pot honeypot on a secure cloud server. T-Pot is a multi-honeypot platform designed to capture, monitor, and analyze malicious activity in real time. 

Honeypots like T-Pot are essential tools in cybersecurity because they provide a controlled environment to study attacker behavior, techniques, and trends without risking production systems. 

The walkthrough covers: 

- Provisioning a cloud server (DigitalOcean droplet) 
- Securing the server with user management and SSH hardening 
- Installing and configuring T-Pot 
- Accessing the T-Pot web interface for monitoring attacks 
- Capturing and visualizing real-world threat activity 

 

By bridging theoretical knowledge with practical application, this project provides a valuable learning platform for cybersecurity students, professionals, and enthusiasts.



## Skills Learned
- Cloud server deployment: Creating and configuring a DigitalOcean droplet with sufficient resources.

- Linux server administration: Updating and upgrading packages, managing users, and applying administrative privileges.

- SSH security and hardening: Generating and deploying SSH key pairs, disabling root login, and securing remote access.

- Honeypot installation and configuration: Cloning and installing T-Pot, and understanding its structure and services.

- Web interface monitoring: Accessing and navigating the T-Pot GUI to observe captured attack activity.

- Data collection and documentation: Recording activity captured by the honeypot over time for analysis.



## Walkthrough
### Step 1: Set Up a Secure Cloud Environment 
To begin, I created a DigitalOcean account and deployed a new droplet to ensure the project was conducted in a secure and isolated environment. The droplet was configured with 4 CPUs, 160 GB of RAM, and 5 TB of storage to provide sufficient system resources for running multiple honeypot services and monitoring tools.
<img width="1597" height="457" alt="Screenshot 2025-10-16 180158" src="https://github.com/user-attachments/assets/561f4ed2-8935-4fbf-9306-375ee523a66e" />

### Step 2: Connect to the Server and Update Packages
After creating the droplet, I established a secure SSH connection to the server using the following command: 

`ssh root@<droplet_public_ip_address>`
 

Once connected, I updated and upgraded all system packages to ensure the environment was up to date: 

`apt-get update && apt-get upgrade -y`

### Step 3: Create a New User with Administrative Privileges
 Next, I created a new user account and granted it administrative (sudo) privileges to follow best security practices and avoid working directly as the root user. 

`adduser <your_username>`
`sudo usermod -aG sudo <your_username>`

After creating the user, I switched to the new account: 

`su <your_username>`


### Step 4: Harden SSH Access 
To enhance server security, I implemented SSH hardening measures. In a separate terminal, I generated a new RSA key pair to enable key-based authentication and reduce reliance on password logins: 

`ssh-keygen -t rsa -b 4096`

 

This step adds an extra layer of protection by ensuring that only users with the corresponding private key can access the server.

### Step 5: Transfer the SSH Public Key to the Server 
After generating the SSH key pair, I transferred the public key to the server to enable secure, passwordless authentication. This was done using the following command: 

`cat ~/.ssh/id_rsa.pub | ssh <your_username>@<your_server_ip> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"`
 

This command creates the .ssh directory on the server (if it doesn’t already exist) and appends the public key to the `authorized_keys file`, allowing access only from systems with the matching private key. 

### Step 6: Configure SSH Settings for Enhanced Security
Next, I edited the SSH configuration file to strengthen server security. 

`sudo vim /etc/ssh/sshd_config`
 

Inside the file, I made the following changes: 

<img width="806" height="93" alt="Screenshot 2025-10-17 190859" src="https://github.com/user-attachments/assets/9d43ae20-04a9-4e57-944a-2d00521a58ed" />

<img width="390" height="76" alt="Screenshot 2025-10-17 190932" src="https://github.com/user-attachments/assets/0c8c1d61-0c1d-43a1-85aa-8896f2d0a963" />


 

Disabling root login and password authentication ensures that only users with valid SSH keys can connect, significantly reducing the risk of unauthorized access. 

### Step 7: Restart the SSH Service 
After modifying the SSH configuration, I restarted the SSH service to apply the new settings: 

`sudo service ssh restart`
 

Restarting the service ensures that all security configurations take immediate effect. 

### Step 8: Clone and Install T-Pot Honeypot 
I began by cloning the official T-Pot repository from GitHub: 

`git clone https://github.com/telekom-security/tpotce`
 

Next, I navigated into the cloned directory and proceeded with the installation: 

`cd tpotce`
Followed by the installation commands:
 
<img width="1920" height="353" alt="Screenshot 2025-10-16 183155" src="https://github.com/user-attachments/assets/22998b2e-0410-41dd-91bd-b5f11a5827e5" />
<img width="1920" height="975" alt="Screenshot 2025-10-16 183812" src="https://github.com/user-attachments/assets/0e74c4c3-0bb0-43e3-8b72-2e49cacc0b7d" />


This step sets up the T-Pot honeypot framework on the server, preparing it to capture and analyze malicious activity in a controlled environment. 

### Step 9: Reboot the System 
After completing the installation, I rebooted the server to ensure all services and configurations were properly initialized: 

`sudo reboot`
 
<img width="1920" height="970" alt="Screenshot 2025-10-16 184003" src="https://github.com/user-attachments/assets/d16777a8-7940-4fdb-9b9e-0b821fade185" />

Rebooting guarantees that T-Pot and its associated components start correctly and that the system is in a clean state before beginning honeypot operations. 

### Step 10: Connect Using the Installer‑Assigned SSH Port 
At the end of the T‑Pot installation, the installer displayed the SSH port assigned for remote access. Using that port, I connected to the server with: 

`ssh -p <installer_assigned_port> <your_username>@<droplet_public_ip_address>`
 

The installer‑assigned port replaces the default port 22. Use the exact port printed in the installation summary (or the installer output) when connecting.

### Step 11: Access the T-Pot Web Interface 
Once connected to the server, I accessed the T-Pot GUI through a web browser using the following URL format: 

`https://<droplet_public_ip_address>:<tpot_gui_port>`
 

The web interface provides a centralized dashboard for monitoring honeypot activity, viewing alerts, and managing the various services running on the T-Pot server. 

### Step 12: Initial Results
Within the first hour of operation, T-Pot began capturing activity. The following screenshots illustrate the dashboard and recorded events: 

<img width="1920" height="864" alt="Screenshot 2025-10-17 200250" src="https://github.com/user-attachments/assets/3b1ad625-9b06-44ca-9c0b-ed07f10e5d69" />

<img width="1920" height="864" alt="Screenshot 2025-10-17 200320" src="https://github.com/user-attachments/assets/611f131b-1ce3-41e8-bf8d-6b33f56b09fa" />

 <img width="1920" height="857" alt="Screenshot 2025-10-17 200331" src="https://github.com/user-attachments/assets/36172e96-aeaf-4c62-9ec1-9febd3008904" />

<img width="1920" height="864" alt="Screenshot 2025-10-17 200341" src="https://github.com/user-attachments/assets/a3e3269c-a6d6-4131-b897-f6480471b17c" />

<img width="1920" height="860" alt="Screenshot 2025-10-17 200354" src="https://github.com/user-attachments/assets/e03fa9cf-415b-4962-ab6d-55ad65102872" />

### Step 13: Results After 48 Hours
After 48 hours of operation, the T-Pot honeypot continued to capture activity. The following screenshots provide an overview of the data collected over this period: 
<img width="1920" height="862" alt="Screenshot 2025-10-19 182658" src="https://github.com/user-attachments/assets/68779191-ed36-49fb-9bba-98daa4381104" />
<img width="1920" height="868" alt="Screenshot 2025-10-19 182708" src="https://github.com/user-attachments/assets/b7d2c04c-d429-41d6-8a8a-062a60fc5c35" />
<img width="1920" height="860" alt="Screenshot 2025-10-19 182725" src="https://github.com/user-attachments/assets/4412cc07-0927-4865-b6cd-57333e04a7b2" />
<img width="1920" height="862" alt="Screenshot 2025-10-19 182750" src="https://github.com/user-attachments/assets/6d1057a8-9f74-476e-9fc0-cc309df68927" />
<img width="1920" height="854" alt="Screenshot 2025-10-19 182802" src="https://github.com/user-attachments/assets/5f96e119-ce38-4e8b-9492-743cea85813b" />

## Conclusion
 This project demonstrates the deployment and operation of a T-Pot honeypot in a secure cloud environment. Over the monitored period, T-Pot successfully captured real-world attack activity, providing valuable insights into threat patterns and attacker behavior. 

By following best practices such as creating a dedicated user, hardening SSH access, and using secure remote connections, the environment remained protected while enabling comprehensive monitoring. 

This setup serves as a foundation for further honeypot experimentation, analysis, and learning in a safe and controlled environment. 





