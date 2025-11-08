## Step One: Installing Node.js, npm, and Nginx
The first thing to do from here is to prepare the system for the Wazuh GUI installalation, which usually involves installing and configuring the wazuh web user interface dependencies; primarily Node.js, npm, and a web server, such as Nginx. The first thing I did, to be safe, was update the package list again:

```
sudo apt update
```

After confirming everything was up to date, the next step was to install Node.js and npm, which are needed for the Wazuh web interface and check the versions to confirm the installation.

```
sudo apt install nodejs npm -y
 node -v
 npm -v
```

After confirming the installation was a success, it was time to install a web server; Wazuh recommends Nginx.

```
sudo apt install nginx -y
 sudo systemctl start nginx
 sudo systemctl enable nginx
 sudo systemctl status nginx
```

## Step Two: Installing and Verifying the Wazuh Dashboard
Now that the manager is installed and running, the next step is to install the Wazuh dashboard. This provides a web-based interface that can be used for monitoring, analyzing, and visualizing the security events that are collected by the Wazuh manager. For this I simply installed, started, enabled it on startup, and checked the status of the dashboard.

```
sudo apt install wazuh-dashboard -y 
 sudo systemctl start wazuh-dashboard
 sudo systemctl enable wazuh-dashboard
 sudo systemctl status wazuh-dashboard
 ```
