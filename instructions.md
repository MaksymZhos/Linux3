Here's the guide in GitHub markdown format:

```bash
# PHSA Application Installation Guide

Follow these steps to install and deploy the PHSA application on your server.

## 1. SSH into the Server
Open your terminal and SSH into the server:
```bash
ssh username@your-server-ip
```
```
Replace `username` with your SSH username and `your-server-ip` with the actual IP address of your server.

## 2. Install MySQL Server
Check if MySQL is installed by running:
```bash
mysql --version
```
If MySQL is not installed, install it using the following commands:

- For Ubuntu/Debian:
  ```bash
  sudo apt update
  sudo apt install mysql-server
  sudo mysql_secure_installation
  ```
- For CentOS/RHEL:
  ```bash
  sudo yum install mysql-server
  sudo systemctl start mysqld
  sudo mysql_secure_installation
  ```

Ensure MySQL is running:
```bash
sudo systemctl status mysql
```

## 3. Install Node.js
Check if Node.js is installed:
```bash
node --version
```
If not installed, use Node Version Manager (nvm):
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
nvm install node
nvm use node
```

Verify installation:
```bash
node --version
npm --version
```

## 4. Create the Application Directory
Create a folder where you want to install the application:
```bash
mkdir /var/www/phsa
cd /var/www/phsa
```

## 5. Clone the Repository
Clone the GitHub repository into your created folder:
```bash
git clone git@github.com:a0a912/phsa.git .
```
Ensure your SSH key is set up on GitHub before running this command.

## 6. Install Project Dependencies
Inside the project folder, install all dependencies:
```bash
npm install
```

## 7. Install PM2
Install PM2 globally:
```bash
npm install pm2@latest -g
```

## 8. Configure Environment Variables
Create a `.env` file in the root of your project directory with the following content:
```env
PORT=4308
FLASK_PORT=4309
DB_HOST=192.18.154.121
DB_USER=do_admin_nj
DB_PASSWORD=1768f3y9hcej0jn31iuy819iji1fmnb972f4gbcuiqwipm
DB_NAME=PHSA
DB_PY_HOST=192.18.154.121
DB_PY_NAME=PHSA
DB_PY_USER=do_read_py
DB_PY_PASSWORD=ye2fivru7498fijcvbu984fgvj9c4fj8ec894f0c940gh9g
DB_manage_HOST=192.18.154.121
DB_manage_NAME=PHSA
DB_manage_USER=do_admin_main
DB_manage_PASSWORD=312435y46u5i7jmynrbvwqd13t45y6u7j43tbg2vf1
```

## 9. Set Up the Database
Run the database setup script:
```bash
python manage_db.py
```

## 10. Install tmux
Install tmux:
```bash
sudo apt-get install tmux
```
For CentOS/RHEL:
```bash
sudo yum install tmux
```

## 11. Start Deployment with tmux
Start a new tmux session:
```bash
tmux new-session -s phsa_deploy
```
Inside the tmux session, run the deployment script:
```bash
python deployment.py
```
Detach from tmux by pressing `Ctrl+b+d`.

## 12. Start the Application with PM2
Start the application using PM2:
```bash
pm2 start server.js
```

## 13. Set Up Reverse Proxy
Install Nginx:
```bash
sudo apt-get install nginx
```
Create a new configuration file for your app:
```bash
sudo nano /etc/nginx/sites-available/phsa
```
Add the following to the file:
```nginx
server {
    listen 80;
    server_name your_domain_or_ip;

    location / {
        proxy_pass http://localhost:4308;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```
Enable the site:
```bash
sudo ln -s /etc/nginx/sites-available/phsa /etc/nginx/sites-enabled/
```
Test and restart Nginx:
```bash
sudo nginx -t
sudo systemctl restart nginx
```

## 14. Verify the Application
Verify that your application is running by visiting your server's domain or IP address:
```bash
http://your-domain-or-ip
```
Check PM2 logs:
```bash
pm2 logs
```
```

