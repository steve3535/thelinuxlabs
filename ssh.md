## Proxy with ssh  
* ensure the port is allowed in /etc/squid/squid.conf with the directive *acl Safe_ports port 22*
* ensure netcat is installed on the host from which u are going to issue the command  
* `ssh -T git@github.com -o 'ProxyCommand nc --porxy 172.22.108.7:80 %h %p'`
   !! Keep %h %p in the command: they are static placeholders
* use case: I want to push to git repo and my remote url is of type ssh, but i am behind a proxy.
          --> Another solution could be to setup https token for the repo in github
