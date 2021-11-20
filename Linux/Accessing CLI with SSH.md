# Accessing CLI & use of SSH keys

## Logging in over the network

The protocol of choice is ssh (secure shell)  
Other protocols exist such as telnet,rsh, rlogin but they are not secure and deprecated !  

* First time you attempt to log, ssh will prompt for the authenticity of the host. The authenticity of the host is materialized as a key fingerprint is stored in the file **.ssh/known_hosts** in your home directory

* SSH key-based authentication:  
  * cloud instances in particular, do not allow login with user/password.  
    Instead you use SSH keys.
  * the password equals to a special file called the private key (to keep secret)
  * the private key matches a public key (which does not need to be secret) and serves as an authorization token on the remote server

* Commands:
  * Generate a SSH key pair:  
  ```ssh-keygen [-f] [keys_dest_path]```  
  By default the keys as saved as **~/.ssh/id_rsa.pub** and **~/.ssh/id_rsa**
  * Sharing the public key:  
  ```ssh-copy [-i] [pubkey_path]``` or copy the **~/.ssh/id_rsa.pub** file manually and save it to the remote file **~/.ssh/authorized_keys**

  * Specify the private key while connecting:  
  ```ssh -i key.pem  <remoteserver>```

## Guided Exercise 01: SSH Key-based auth

* connect to your workstation as student

* generate a key pair to be used as auth on serverb with user operator

* generate a second key pair to be used as auth on serverc with user root

* from your workstation and with ssh, display the date and time on the remote serverc  


