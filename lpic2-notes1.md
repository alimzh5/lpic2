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
