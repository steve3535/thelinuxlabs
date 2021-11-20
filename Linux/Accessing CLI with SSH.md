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

  * When working with several accounts and several servers, it may be very useful to control your SSH Key-based through a configuration file: **~/.ssh/config**

    ```bash
    Host servera.thelinuxlabs.com
         Hostname 192.168.122.1
         User operator
         IdentityFile /home/student/mykeys/private.key
    ```

## Guided Exercise 01: SSH Key-based auth

* connect to your workstation as student

* generate a key pair to be used as auth on serverb with user operator

* generate a second key pair to be used as auth on serverc with user root

* from your workstation and with ssh, display the date and time on the remote serverc  

## Assignement  

GitHub, one of the most collaborative platforms allows ssh key based operations on defined repositories.  
However a public SSH key cannot be shared among repositories.  
This means each repository has its own unique key to be used with.  

* Signup to github.com (if you are not already registered)

* Activate the 2FA setting on your account

* Create two distinct new repos

* On your workstation, setup two directories to serve as local branches for your git repos

* Enable SSH Key-based for the two repos and ensure push/pull is working for each of them without prompting for a password  

### Annex

1. Getting started with Github [github tuto](https://docs.github.com/en/get-started/quickstart/hello-world)

2. Use git CLI


