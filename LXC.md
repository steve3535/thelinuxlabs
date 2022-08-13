### LXD containers not getting IPV4 address dynamically

I don't know if this is something conflict between vmware and libvirt running concurrently on my server  
```bash
lxc image copy images:xxxxxx local: --alias ubuntu
lxc launch ubuntu
lxc ls
```
I dont see any ip address  
even after loging in with *lxc shell ctr_name* and *dhclient eth0*, still nothing  
cheks on the LXD network seem normal:  
```bash
lxc network ls 
lxc network show lxdbr0
brctl show
ip a
``` 
changed the distro to centos , but still  
Further checks in the containers:  
**dhclient -v eth0** --> **DHCPDISCOVER on eth0 to 255.255.255.255 port 67 interval** and **No DHCPOFFERS received** error messages  
the above can  also been seen in the container with **journalctl -xe** in fedora based distros  

**ss -tlpn** or **ss -ulpn**  

#### Solution
Normally, upon a dhcp discover (especially since it runs on the same host), dhcp reply should be immediate.  
When I run tcpdump on the host:  
> **tcpdump -i lxdbr0 port 67**  
> dropped privs to tcpdump  
> tcpdump: verbose output suppressed, use -v or -vv for full protocol decode  
> listening on lxdbr0, link-type EN10MB (Ethernet), capture size 262144 bytes    
> 13:40:27.155773 IP 0.0.0.0.bootpc > 255.255.255.255.bootps: BOOTP/DHCP, Request from 00:16:3e:ff:47:f4 (oui Unknown), length 284  
> 13:41:16.156386 IP 0.0.0.0.bootpc > 255.255.255.255.bootps: BOOTP/DHCP, Request from 00:16:3e:ff:47:f4 (oui Unknown), length 300  
> 13:41:19.405042 IP 0.0.0.0.bootpc > 255.255.255.255.bootps: BOOTP/DHCP, Request from 00:16:3e:ff:47:f4 (oui Unknown), length 300  
> 13:41:23.905714 IP 0.0.0.0.bootpc > 255.255.255.255.bootps: BOOTP/DHCP, Request from 00:16:3e:ff:47:f4 (oui Unknown), length 300  

No reply response ...   
meanwhile in the container:  
> **dhcpclient -v eth0**  
> DHCPDISCOVER on eth0 to 255.255.255.255 port 67 interval 3 (xid=0xe3688531)  
> DHCPDISCOVER on eth0 to 255.255.255.255 port 67 interval 4 (xid=0xe3688531)  
> DHCPDISCOVER on eth0 to 255.255.255.255 port 67 interval 7 (xid=0xe3688531)  

--> conclusion: the host is not replying or smething is preventing it to .. firewall ? 🙂   
I discovered this link and that was it ! --> https://linuxcontainers.org/lxd/docs/master/howto/network_bridge_firewalld/   
In summary:  
* I disabled the firewalld to see if it works and it worked  
* I reenable the firewalld and run the below:  
  ```bash
  lxc network set lxdbr0 ipv4.firewall false
  firewall-cmd --zone=trusted --change-interface=lxdbr0 --permanent
  firewall-cmd --reload
  ``` 
  


