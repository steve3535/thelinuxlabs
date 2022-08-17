### Bridging with external network (in a ESXi like deployment)  
1. Follow the steps strictly to configure the VM on ESX   
   https://developer.cisco.com/docs/modeling-labs/#deploying-the-ova-on-esxi-server  

   In summary for the network:  
   
   >To use External Connectivity with bridged networking, you should also configure the port group and vSwitch that’s used by the CML VM to set:  
   >Promiscuous Mode = Accept  
   >Forged Transmits = Accept  
   >Additionally, on the ESXi host where the CML VM runs, configure the Advanced System Settings to set  
   >Net.ReversePathFwdCheck = 1  
   >Net.ReversePathFwdCheckPromisc = 1  


2. Add an unmanaged switch and an external connection of type bridge 


