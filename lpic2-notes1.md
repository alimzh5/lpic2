# Network Interface Configuration Example (Static and Dynamic IP)
```
# The primary network interface configuration

# This interface (enp0s3) uses a STATIC IP address
auto enp0s3                     # Automatically bring up the interface at boot
iface enp0s3 inet static        # Define the interface to use IPv4 with a static address
    address 192.168.56.10       # Assign a fixed IP address
    netmask 255.255.255.0       # Define the subnet mask

# This interface (enp0s8) uses DHCP (dynamic IP)
auto enp0s8                     # Automatically bring up the interface at boot
iface enp0s8 inet dhcp          # Obtain IP automatically via DHCP
```
***
# Installing and Checking OpenSSH Server (for SSH access)
```
# Check if OpenSSH Server is installed
dpkg -l | grep openssh-server

# If it's not installed, install it using apt-get
sudo apt-get install openssh-server
```
## Explanation:

* dpkg -l → Lists all installed packages on the system.

* | grep openssh-server → Filters the list to show only results containing "openssh-server".

* sudo apt-get install openssh-server → Installs the OpenSSH Server package, which allows remote SSH connections to your machine.
***
# Checking and Editing SSH Configuration File
```
# Check if OpenSSH Server is installed
dpkg -l | grep openssh-server

# Go to the SSH configuration directory
cd /etc/ssh

# List files in the directory
ls

# Edit the SSH server configuration file
vi sshd_config
```
## Explanation:

* dpkg -l | grep openssh-server → Checks if the OpenSSH server package is installed.

* cd /etc/ssh → Changes directory to where SSH configuration and key files are stored.

* ls → Lists all files in the /etc/ssh directory.
You will see files like:

    * sshd_config → main SSH server configuration file

    * ssh_host_rsa_key, ssh_host_ecdsa_key → server’s private keys

    * .pub files → public keys

* vi sshd_config → Opens the SSH configuration file in the vi text editor.

## Changing the SSH Port

Inside the sshd_config file, you’ll find a line like this:
```
Port 22
```

* This means the SSH service is listening on port 22 (the default port).

* If you want to change it (for security or customization), you can modify it, for example:
```
Port 2222
```

After saving the file, restart the SSH service for changes to take effect:
```
sudo systemctl restart ssh
```
***
# Verifying SSH Service Listening Port
```
# Check if SSH is listening on port 22
netstat -ntulp | grep :22
```
## Explanation:

* netstat → Shows network connections, routing tables, and listening ports.

* -n → Displays addresses and port numbers in numeric form.

* -t → Shows TCP connections.

* -u → Shows UDP connections.

* -l → Displays only listening ports.

* -p → Shows the program (process) using the port.

* | grep :22 → Filters output to show only lines containing “:22” (the SSH port).

When you see output like this:
```
tcp   0  0 0.0.0.0:22     0.0.0.0:*     LISTEN   12239/sshd
tcp6  0  0 :::22          :::*          LISTEN   12239/sshd
```

It means the SSH daemon (sshd) is actively listening on port 22 for both IPv4 and IPv6 connections.
***
# Connecting to SSH Server Using PuTTY
## Step 1 — Open PuTTY

After installing PuTTY, open the program.
You’ll see the PuTTY Configuration window.

## Step 2 — Enter Connection Information
```
Host Name (or IP address): 192.168.56.10
Port: 22
Connection type: SSH
```

## Explanation:

* Host Name / IP address → Enter the IP address of the Linux machine you want to connect to.

* Port → The SSH port (default is 22, or the new port if you changed it).

* Connection type → Select SSH.

Then click Open to start the connection.

## Step 3 — Login

When the terminal opens, you’ll be asked to log in:
```
login as: root
root@192.168.56.10's password:
```

Or you can log in with your normal user account.

# Enable Root Login (if needed)

By default, SSH does not allow root login for security reasons.
If you want to log in directly as `root`, follow these steps:

Open the SSH configuration file:
```
sudo vi /etc/ssh/sshd_config
```

Find the line:
```
PermitRootLogin prohibit-password
```

or it might be:
```
PermitRootLogin no
```

Change it to:
```
PermitRootLogin yes
```

Save and restart the SSH service:
```
sudo systemctl restart ssh
```

Now you can connect to your Linux machine using your root account in PuTTY.
***
# Connecting to SSH Server Using Windows CMD or PowerShell

You can also connect to your SSH server directly from Command Prompt (CMD) or PowerShell, without using PuTTY.

## Step 1 — Open CMD or PowerShell

Press Win + R, type:
```
cmd
```

or search for PowerShell, then press Enter.

## Step 2 — Use the ssh Command

The general syntax is:
```
ssh username@IP_address
```

For example:
```
ssh root@192.168.56.10
```

Then you’ll be asked for the password:
```
root@192.168.56.10's password:
```

## Explanation:

* `ssh` → Command to start an SSH session.

* `root` → The username you want to log in with.

* `192.168.56.10` → The IP address of your Linux system.

## If You Changed the Port

If your SSH server is running on a different port (for example `2222`), you can specify it like this:
```
ssh -p 2222 root@192.168.56.10
```
***
# Linux Network Configuration – Summary Notes
## File Location:
```
/etc/network/interfaces
```
## Purpose:

Defines how Linux network interfaces are configured (manually or automatically).

## Key Directives:
Directive	Meaning

`auto <iface>`	Brings the interface up automatically at boot.

`iface <iface> inet static`	Configures a static (manual) IP.

`iface <iface> inet dhcp`	Configures dynamic IP (gets it from a DHCP server).

`address`	Assigns an IP address.

`netmask`	Defines the subnet mask.

## Example Configuration:
```
# Loopback interface
auto lo
iface lo inet loopback

# Main static network card
auto enp0s3
iface enp0s3 inet static
    address 192.168.56.10
    netmask 255.255.255.0

# Alias interface (virtual IP on the same card)
auto enp0s3:0
iface enp0s3:0 inet static
    address 192.168.56.100
    netmask 255.255.255.0

# Secondary adapter with DHCP
auto enp0s8
iface enp0s8 inet dhcp
```

## Notes:

`lo` = loopback (localhost, 127.0.0.1)

`enp0s3` = main adapter with static IP

`enp0s3:0` = alias interface (adds an extra IP to the same NIC)

`enp0s8` = gets IP automatically via DHCP

## Apply Configuration:

After editing the file, restart networking service:
```
sudo systemctl restart networking
```

or
```
sudo service networking restart
```
## Quick Check:
```
ip addr
```

→ View assigned IPs.
```
ping 192.168.56.10
```

→ Test connection.
***
