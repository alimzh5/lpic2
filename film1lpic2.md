#Network Interface Configuration Example (Static and Dynamic IP)
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
