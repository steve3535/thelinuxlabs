## VMWARE on RHEL 8.6 on DL385

### TL;DR
I don't even remember what I did to get there: there are so many glitches getting VMware running on Linux.  
Some key points:  
* check [this guy](https://github.com/mkubecek/vmware-host-modules/) for the specific version you are trying to install
* a useful forum link that helped me once on Pop'OS: https://communities.vmware.com/t5/VMware-Workstation-Pro/VM-Workstation-16-1-gt-16-2-1-on-Ubuntu-21-10-broke-everything/m-p/2885428
* VMware Workstation 16.x.x Pro license key that worked for me: **ZF3R0-FHED2-M80TY-8QYGC-NPKYF**  
* VMware ESXi license key = **JM0C3-4C082-M8VG0-02AU2-0NKMH**  
* to have the Vmware workstation displayed as gui:  
  since I am running everything from my laptop which is running X:  
  ```bash
     ssh -x root@dl385
     vmware &
  ```
 * I needed Vmware because of CML and in order to run some real world scenarios like Ansible+vcenter/vmware   
 
## INSTALL and setup CML

I decided to go for deployment of CML in ESX  
main reason was that I wanted to play with things like trunking and use the CML routers to segregate group of servers.  
So instead of later fihuring out how to link CML to KVM hosts, better have them within the same hypervisor.  

### cannot open the OVA file of CML to begin the install 
Was failing without any error message on the screen  
checking on the log file locate in **/tmp/vmware-root**: **vmware-ui.log**, i found that it was complaining about the *OVFtool*  
Tried run **ovftool** command on the CLI --> lead to an error saying *missing dependecy **libnsl***  
Solution:
`dnf -y install libnsl`   

### Creds and Access
>**CML**  
> admin/sysadmin  
> sysadmin/sysadmin

>**ESX**  
> root/MyNameIsKw@kou1981#  
> root/;yNq;eIsKz2kou19813  

With ESX, the network mode chosen is NAT (note that bridge mode worked perfectly in Vm Workstation making a router with ext. connection to be directly reachable in the 192.168.178.x physical subnet)  
I have to insert iptables rules for the CML and ESX services:  
```bash
   iptables -t nat -A PREROUTING -p tcp --dport 8088 -j DNAT --to-destination 172.16.68.128:443  
   iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 172.16.68.129:443
   iptables -t nat -A POSTROUTING -j MASQUERADE 
```
172.16.68.128 being ESXi web portal and 172.16.68.129 being CML portal 
I can then access them from my personal PopOs laptop with **https://192.168.178.40:8088** and **https://192.168.178.40:8080** respectively  
192.168.178.40 being my DL385 ip address  

**ATTENTION: POUR FAIRE DU NAT FORWARDING VERS UN GUEST KVM**  
Dans ce cas, cest différent: il faut insérer des règles de FORWARD aussi (ie en plus des regles PREROUTING, POSTROUTING etant facultatif dans ce cas):  
```bash
iptables -I FORWARD -i eno1 -o virbr0 -d 192.168.122.0/24 -j ACCEPT
iptables -I FORWARD -i virbr0 -o eno1 -s 192.168.122.0/24 -j ACCEPT
```
eno1 etant linterface du server (donc en queque sorte son wan adapter)  

Quelquechose dintéressant a tester: dans le cas dun traffic web, au lieu de faire du iptables, utiliser un reverse proxy nginx  


## INCREASE CAPACITY WITH HP ACU  
My challenge is I dont have a VGA screen i can attach to the server in other to go into HP ACU and econfigure the array.  
recall I have 8 disks SSD of 400gb each and i only configured 1 array of 4  
I needed to find a way of creating logical drive on the smart ctl from within the OS  
* `wget http://downloads.linux.hpe.com/SDR/add_repo.sh`  
* `./add_repo.sh -d CentOS -r 8 mcp`  
* `dnf -y install ssacli` 
* `ssacli` 
> help  
> controller all show  
> ctrl slot=0 show  
> ctrl slot=0 physicaldrive all show  
> ctrl slot=0 logicaldrive all show 
> ctrl slot=0 create type=ld drives=2I:2:5-2I:2:8 raid=0  

After that: **lsblk -a** should show a new disk drive ready to be used !  




