# Linux-Mandatory-assignement2
Setting up unprivileged two containers

Hi.
We have created two unprivileged containers (C1, C2) C2 has the rand.numbers script and C1 can fetch random numbers
from the C2. 
We have forwarded the network connecting from the PI's port 80 to C1's port 80.
unlike the other groups we have forwarded wlan0 instead of usb0 since our PI is setup thru wlan.
it seems to work as intended, but sometimes the static IP's of C1 and C2 is bugging, and they get a random IP adress

Best regards group 6.




BELOW YOU WILL FIND COMMANDS USED ETC: -----------------------------------------------

To fetch the random numbers from C2 the following command was used:

echo getRand | socat - tcp:10.0.3.12:8080

C2 listen command:

socat -v -v tcp-listen:8080,fork,reuseaddr exec:/bin/rng.sh

/bin/rng.sh script:

#!/bin/ash

dd if=/dev/random bs=4 count=16 status=none | od -A none -t u4

STATIC IP FOR CONTAINERS: -------------------------------------------------------------

Create /etc/lxc/dhcp.conf

dhcp-host=C1,10.0.3.11

dhcp-host=C2,10.0.3.12
  
  

RESTART LXC-NET: ---------------------------------------------------------------------
  
systemctl restart lxc-net
  
  
  
USEFUL COMMAND FOR SETUP AND DEBUGGING: ---------------------------------------------

  
lxc-stop -n containerName

lxc-start -n containerName

lxc-attach -n containerName (will go "into" container)

lxc-destroy -n containerName (destorys the container)

exit (will exit the container)
  
lxc-ls --fancy OR lxc-ls -f

  This returns: name, state and IP
  
  This is done to check if contrainer is running or stopped and if static IP is 
  working as inteded

sysctl net.ipv4.ip_forward=1 (This need to be set to 1, to enable forwarding of ip from PI to container)

(Rerouting from PI's port 80 to C1:80)

iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 80 -j DNAT --to-destination 10.0.3.11:8000  


