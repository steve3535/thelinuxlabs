## Limitations of cloud images
* when using virt-install, we usually do use the cloud kvm image of ubuntu: it is a minimal kernel image that doesnt contain all modules, for e.g. nfsd
  * i wanted to start an nfs-server on my instance and found that the `systemctl status nfs-server` exits with dependency error
  * with `systemctl cat nfs-server` --> quickly found that the issue was with *proc-fs-nfsd.mount* unit
  * check if the nfsd module was loaded: `lsmod | grep nfsd`; `modprobe nfsd` --> exits with code 32 (unable to mount) because:
    >modprobe: FATAL: Module nfsd not found in directory /lib/modules/5.15.0-1004-kvm
    5.15.0.1004-kvm being my current kernel
    `find /lib/modules/ -name "nfsd.ko"` confirms its missing
  * one solution was to get the generic image (the normal one) and switch to that kernel:
    * `apt install linux-generic`
    * `grep menuentry /boot/grub/grub.cfg` --> get the id of the generic kernel,something similar to: gnulinux-advanced-0522e6b3-8d40-4754-a87e-5678a6921e37>gnulinux-generic-xxxxx-yyyy-zzzz (Advanced id>image id)
    * set tha id for GRUB_DEFAULT in /etc/default/grub
    * `update-grub`
    * reboot
  * I ended up with a system panic :)
  * Tried another format: GRUB_DEFAULT="1>4" but still  (rememeber counting from 0)

 
    
      
