# Linux a3p2 Adding a Firewall

## Step 1: Check if ufw is installed and ssh is alowed, as you dont want to be locked out of the system.

### 1.1 Check for updates and update if needed your existing repositories.

```bash
sudo pacman -Syu
```
### 1.2 Install ufw using pacman.

```bash
sudo pacman -S ufw
```
### 1.3 Check current state of the ufw.

```bash
sudo ufw status #to check the current active state
sudo ufw show added to see which ufw are set
```

- There should be no rules set and the state should be inactive. DO NOT ENABLE before completing next steps as it will resault in losing ablity to connect to your system!

### 1.4 Allow the use of needed ports with the following commands.

```bash
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow 8080
```

- Check if all the rules applied usnig.

  ```bash
  sudo ufw show added
  ```
- You should see the following output.
  ![image](https://github.com/MaksymZhos/Linux3/assets/148744478/94b2f78d-cdb2-4c41-8a10-0438efe2e7d9)

### 1.4 Now we can enable ufw using.

```bash
sudo ufw enable
```

- To ensure everything works run.

  ```bash
  sudo ufw status
  ```
- You should see the following output.
![Screenshot 2024-04-10 125626](https://github.com/MaksymZhos/Linux3/assets/148744478/9616352e-19c7-4e73-b011-8e0ca7cab62b)


# Step 2: Setting up Server File

### 2.1 Download the file provided alongside instructions and open file location in terminal on your local machine.

### 2.2 With your powershell in that directory, use sftp to connect to your droplet and upload the file using the put command through sftp. 

```bash
sftp linux
```
- After running the command above you should see similar output which will vary depended on your file location.
  
  ![image](https://github.com/MaksymZhos/Linux3/assets/148744478/18ed7f14-dbf8-4bcf-b274-861cd5fb50fa)
- Next type and run.
```bash
put hello-server
```
- You should see the following output.
![Screenshot 2024-04-10 130650](https://github.com/MaksymZhos/Linux3/assets/148744478/edf45d8b-eff1-4f53-ba04-8b916c3faaa4)


### 2.3  Move the file to the correct location.

```bash
sudo mv hello-server /usr/local/bin/backend
```

### 2.4 Make the file into executable.

```bash
sudo chmod u+x /usr/local/bin/backend
```

### 2.5.1 Open file to set its properties as a service so it can correctly run in the background.

In this example we are using vim.
```bash
sudo vim /etc/systemd/system/backend.service
```
### 2.5.2 Copy and paste the folloing into the file.

```bash
[Unit] #This gives information about the service
Description=Backend Service #Name of the service
After=network.target

[Service]
Type=Simple
ExecStart=/usr/local/bin/backend #Location of the File
Restart=always #Reload after changes

[Install]
WantedBy=multi-user.target #Works for all users
```
### 2.5.3 Start and Enable the service. 

```bash
sudo systemctl start backend
sudo systemctl enable backend
```

# Step 3: Reverse Proxy

### 3.1 Open the configuration file you made in sites-available last assignment.

```bash
sudo vim /etc/nginx/sites-available/nginx-2420
```
### 3.2 Copy and paste the following lines into the server block.
```bash
location /hey {
    proxy_pass http://127.0.0.1:8080;
}

location /echo {
    proxy_pass http://127.0.0.1:8080;
}
```

### 3.3 Run ``` sudo nginx -t ``` to ensure correct syntax.

# Testing

- Run ``` sudo systemctl restart nginx ``` in order to restart.

- You can now  go to ``` http:/your-droplet-ip/hey ``` or ``` http:/your-droplet-ip/echo ``` using your postman and sending get requests.
- Here are the examples of running server
-  ![image](https://github.com/MaksymZhos/Linux3/assets/148744478/d9a045b5-b906-44aa-825e-9c46eeabbde7)
-  ![image](https://github.com/MaksymZhos/Linux3/assets/148744478/d1d2ece0-2881-4f23-a8bb-f07718bbb4a6)
-  ![image](https://github.com/MaksymZhos/Linux3/assets/148744478/ad1b18d7-a5c6-4da7-87ba-c15f1cda054c)


 

  
