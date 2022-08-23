### EBS
* 1 gb < size < 16 tb
* bound to AZ  
  * workwround this is to make snapshot and move it across  
* if not root volume, it is not set to delete upon termination by default  
* EBS = network drive --> can be attached/detached any how , --> there might be some latency when accessing the drive  
* can only be mounted at one instance at at time (CCP level)  -- except with io1/io2 ebs types
* EC2 instance store vs EBS == ephemeral vs persistent == very high performance vs moderate perf == hardware(phys. hdd) attached vs network attached     
* EC2 instance store, for root vol, is billed per instance and snapshot(AMI), not for volume usage  
* EBS volume types: gp2/gp3, io1/io2, st1/sc1  
* AMI determines the type of root vol config: either ebs backed or ec2 instance store backed  
* EC2 instance store can go up to 3M iops for certain instance types !!!  
* initial iops for io1/io2 = 16K, can go up to 32K and 64K for nitro type instances
