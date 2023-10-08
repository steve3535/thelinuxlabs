## Common commands
* Shutdown a VM: `virsh destroy node1` or `virsh shutdown node1`
* Delete a VM: `virsh undefine node1 --remove-all-storage`
* Take a snapshot: `virsh snapshot-create-as --domain node1 --snapshotname s-node1-07102023`
* List snapshots: `virsh snapshot-list --domain node1`
* Restore a snapshot: `virsh snapshot-revert --domain node1 --snapshotname s-node1-07102023`
  
## virt-install console issue with ubuntu full image
* situation: trying to install ubuntu with kvm in cli:
  ```bash
  virt-install --name node --vcpus 1 -r 2048 --disk path=/var/lib/libvirt/images/node.qcow2,bus=virtio --network default,model=virtio \
  --cdrom ubuntu23.04.iso --os-variant ubuntu22.04 --graphics none
  ```
  The issue here is that the console does not work.
  And to specify that the console should be redirected to the serial line (here our terminal), we need extra-args (only possible with --location instead of --cdrom)  
  PS: --location fits for urls
  ```bash
  mount -o loop ubuntu23.iso /media
  virt-install --name node --vcpus 1 -r 2048 --disk path=/var/lib/libvirt/images/node.qcow2,bus=virtio --network default,model=virtio \
  --location /media --os-variant ubuntu22.04 --graphics none --extra-args 'console=ttyS0,115200n8 serial' --console pty,target_type=serial
  ```
  Unfortunately there seems to be an issue with the new ubuntu ISOs as they dt expose any directory where u can find vmlinuz and initrd
  --> for e.g. I checked for version 18.04 and it is present, hence for new versions --location triggers errors that it cant see the installation tree
  ### SOLUTION
  1. `mount -o loop ubuntu23.04.iso /media`
  2. the kernel and initram fs are under the folder /media/casper
  3. instead of --extra-args, use --boot and pass kernel params
  4. ```bash
     virt-install --name node --vcpus 1 -r 3072 --network default,model=virtio --disk path=/var/../nde3.qcow2,bus=virtio,size=20 \
     --cdrom ubuntu23.04.iso --console pty,target_type=serial --graphics none --boot kernel=/media/casper/vmlinuz,initrd=/media/casper/initrd,kernel-args="console=ttyS0"
     ```
  5. virsh destroy node3
  6. virsh edit node3 and 
     remove <boot>,<kernel> and <initrd> lines
     
  
  
  
