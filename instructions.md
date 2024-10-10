Here is the entire process as a single Markdown file, combining all steps and details:


# Cloud Deployment on Oracle Cloud (25%)

## Step 1: Setting Up Cloud Infrastructure

### Task:
- I used **Oracle Cloud** for this project.
- Launched an Oracle Linux instance and installed necessary dependencies.

### Security Configuration:
- Configured security groups (firewall rules) to allow HTTP and HTTPS traffic by setting up Ingress rules in the Oracle Cloud firewall. These rules enabled traffic on ports 80 (HTTP) and 443 (HTTPS) to reach the cloud instance.

---

## Step 2: Installing Caddy on the Cloud Instance

### Task:
Install Caddy on the Oracle Cloud instance.

### Steps:

1. **SSH into the instance**:
   ```bash
   ssh -i your_key.pem opc@<public_ip_of_instance>
   ```

2. **Enable COPR Repository**:
   ```bash
   sudo dnf install 'dnf-command(copr)'
   sudo dnf copr enable @caddy/caddy
   ```

3. **Install Caddy**:
   ```bash
   sudo dnf install caddy
   ```

4. **Start and enable Caddy**:
   ```bash
   sudo systemctl start caddy
   sudo systemctl enable caddy
   ```

5. **Verify Caddy Installation**:
   ```bash
   caddy version
   ```

### Making Caddy Publicly Accessible:
- I made Caddy publicly accessible by allowing HTTP and HTTPS traffic through the Oracle Cloud firewall and correctly setting the domain name in the Caddyfile configuration.

---

## Step 3: Domain Name and HTTPS Setup

### Task:
Configure a domain name and enable HTTPS for the website using Caddy.

### Steps:

1. **Get a Domain**:
   - I registered a domain using a free service like **Freenom**. The domain I used is `vanila.lynxserver.games`.

2. **Configure DNS A Record**:
   - Created an A record to point the domain to the public IP address of the Oracle Cloud instance.
     - **Name**: `vanila.lynxserver.games`
     - **Type**: A
     - **Value**: `<instance_public_ip>`

3. **Configure Caddyfile**:
   - Edited the Caddyfile to handle automatic HTTPS and set up a reverse proxy to forward requests to the web app running on port `3001`.

   **Caddyfile**:
   ```text
   vanila.lynxserver.games {
       reverse_proxy localhost:3001
       log {
           output file /var/log/caddy/access.log
       }
   }
   ```

4. **Restart Caddy**:
   ```bash
   sudo systemctl restart caddy
   ```

5. **Check Caddy Logs** for any issues:
   ```bash
   journalctl -u caddy --no-pager | tail -n 50
   ```

---

## Step 4: Deploying the Website

### Task:
Deploy the web application from Part 2 (login and session functionality) to the Oracle Cloud instance.

### Steps:

1. **Install PM2 to Manage the Application**:
   ```bash
   sudo npm install -g pm2
   ```

2. **Start the Node.js Application Using PM2**:
   ```bash
   pm2 start app.js --name "myApp"
   pm2 save
   pm2 startup
   ```

3. **Ensure Caddy is Configured for Reverse Proxy**:
   - The Caddyfile routes all traffic coming to `vanila.lynxserver.games` to the Node.js application running on port `3001`.

4. **Verify Website Deployment**:
   - Verified that the login and session functionality of the website works properly via the domain name `vanila.lynxserver.games`.

---

## Troubleshooting Steps:

### Common Issues:
- **DNS Propagation**: If the domain is not resolving, ensure the A record points to the correct public IP address and wait for DNS changes to propagate (which can take up to 24 hours).
- **Caddy SSL Issues**: If SSL fails to work, check the Caddy logs for potential errors:
  ```bash
  journalctl -u caddy --no-pager | tail -n 50
  ```

### Solutions:
- Restarted Caddy after making DNS changes to ensure the configurations were applied correctly.
- Ensured that ports 80 and 443 were open in the Oracle Cloud firewall.

---

## Conclusion:

The website is successfully deployed on the Oracle Cloud instance with automatic HTTPS configured using Caddy. The website is fully operational and accessible via the domain `vanila.lynxserver.games`, with login and session functionalities working as expected.
```

Make sure to add the relevant screenshots alongside the Markdown file, as mentioned in the report!
