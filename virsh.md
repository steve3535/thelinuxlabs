* Shutdown a VM: `virsh destroy node1` or `virsh shutdown node1`
* Delete a VM: `virsh undefine node1 --remove-all-storage`
* Take a snapshot: `virsh snapshot-create-as --domain node1 --snapshotname s-node1-07102023`
* List snapshots: `virsh snapshot-list --domain node1`
* Restore a snapshot: `virsh snapshot-revert --domain node1 --snapshotname s-node1-07102023`
  
