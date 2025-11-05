## Step One: Updating and Upgrading Packages
The first thing I did was run a couple simple commands to make sure everything on the VM is up to date before installing dependencies:

```
sudo apt update
sudo apt upgrade -y
```

### Command Breakdown:
⦁ `Sudo` - temporarily gives superuser privileges to allow system-level changes
⦁ `apt` - package management tool for Debian-based systems, such as Ubuntu
⦁ `update` - refreshes the local list of available packages and their versions
⦁ `upgrade` - installs the latest versions of installed packages
⦁ `-y` - automatically answers yes to any confirmation prompts

## Step Two: Installing Required Utilities
This installs common utilities (curl, wget, unzip, etc.) that Wazuh relies on:

```
sudo apt install curl apt-transport-https unzip wget gnupg -y
```

### Command Breakdown:
⦁ `install` - tells apt to download and install specified packages
⦁ `curl` - transfers data to or from a server, often used for downloading scripts
⦁ `apt`-transport-https - allows apt to access repositories over HTTPS
⦁ `unzip` - extracts .zip archives
⦁ `wget` - another tool for downloading files from the web
⦁ `gnupg` - provides encryption and signing capabilities for verifying repository keys

## Step Three: Verify Everything is Installed Correctly
I used this command to confirm that all necessary utilities were installed:

```
ls -la /usr/bin | grep -E "curl|wget|unzip"
```

### Command Breakdown:
⦁ `ls -la /usr/bin` - lists all files in /usr/bin including hidden files
⦁ `|` - pipes the output from one command into the next
⦁ `grep -E` - searches text using extended regular expressions
⦁ `"curl|wget|unzip"` - matches any line containing curl, wget, or unzip

## Step Four: Importing Wazuh GPG Key
Next, I imported the Wazuh GPG key so Ubuntu can trust the packages:

```
curl -s https://packages.wazuh.com/key/gpg-key-wazuh
 | sudo gpg --dearmor -o /usr/share/keyrings/wazuh.gpg
```

### Command Breakdown:
⦁ `curl -s` - downloads the GPG key silently
⦁ `gpg --dearmor` - converts the key into a format apt can use
⦁ `-o /usr/share/keyrings/wazuh.gpg` - saves the key in the system keyring directory

## Step Five: Add the Wazuh Repository
This command tells Ubuntu where to find Wazuh packages:

```
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt
 stable main" | sudo tee /etc/apt/sources.list.d/wazuh.list
```

### Command Breakdown:
⦁ `echo` - prints the repository line
⦁ `[signed-by=/usr/share/keyrings/wazuh.gpg]` - ensures apt trusts packages signed with this key
⦁ `| sudo tee /etc/apt/sources.list.d/wazuh.list` - writes the line into a new file

## Step Six: Update the Package List
Now that the repository is added, update the package list to see the Wazuh packages:

```
sudo apt update
```

## Step Seven: Installing the Wazuh Manager
The Wazuh manager collects, analyzes, and correlates security events. The API is included in the manager package:

```
sudo apt install wazuh-manager -y
```

## Step Eight: Start and Enable Wazuh Services
Start the Wazuh manager, enable it to launch at boot, and verify it is active:

```
sudo systemctl start wazuh-manager
sudo systemctl enable wazuh-manager
sudo systemctl status wazuh-manager
```

### Command Breakdown:
⦁ `systemctl` - tool for controlling systemd services
⦁ `start` - launches the service immediately
⦁ `enable` - sets the service to start automatically at boot
⦁ `status` - shows whether the service is running

## Step Nine: Verify Wazuh Manager is Active and Collecting Logs
Finally, check that Wazuh is processing logs properly:

```
sudo tail -n 20 /var/ossec/logs/ossec.log
sudo tail -f /var/ossec/logs/ossec.log
```

### Command Breakdown:
⦁ `tail -n 20` - shows the last 20 lines of the log, providing a quick view of recent activity
⦁ `tail -f` - follows the log in real time, ideal for confirming ongoing log collection and troubleshooting
