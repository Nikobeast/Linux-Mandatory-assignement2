# Lunux-Mandatory-assignement2
Setting up unprivileged two containers

Hi.
Our Solution doesnt work 100% we have troubles connecting it to the outside (web), we have come to the point where when
we try to acces it remotely from the web we get 404:error page not found
We dont know where it went wrong. 
But we have created two unprivileged containers (C1, C2) C2 has the rand.numbers script and C1 can fetch random numbers
from the C2. 
We have forwarded the network connecting from the PI's port 80 to C1's port 80.
unlike the other groups we have forwarded wlan0 instead of usb0 since our PI is setup thru wlan.
it seems to work as intended, but we have trouble finding out where it went wrong.

Best regards group 6.




BELOW YOU WILL FIND COMMANDS USED ETC: -----------------------------------------------

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
exit (will exit and container)
  
lxc-ls --fancy OR lxc-ls -f
  This returns: name, state and IP
  This is done to control if contrainer is running or stopped and if static IP is 
  working as inteded

sysctl net.ipv4.ip_forward=1 (This need to be set to 1, to enable forwarding of ip from PI to container)

iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 80 -j DNAT --to-destination 10.0.3.11:8000  (Rerouting from PI 80 to C1:8000)


