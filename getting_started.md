## VMWARE on RHEL 8.6 on DL385

### TL;DR
I don't even remember what I did to get there: there are so many glitches getting VMware running on Linux.  
Some key points:  
* check [this guy](https://github.com/mkubecek/vmware-host-modules/) for the specific version you are trying to install
* a useful forum link that helped me once on Pop'OS: https://communities.vmware.com/t5/VMware-Workstation-Pro/VM-Workstation-16-1-gt-16-2-1-on-Ubuntu-21-10-broke-everything/m-p/2885428
* VMware Workstation 16.x.x Pro license key that worked for me: **ZF3R0-FHED2-M80TY-8QYGC-NPKYF**  
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