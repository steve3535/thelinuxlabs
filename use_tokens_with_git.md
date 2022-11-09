## https token over git for authentication to a remote github repo  
### situation (S of STAR)  
>i was doing documentation, based on some work i did at office  
pb is that i am sitting at home while doing it  
so, I launched my remote desktop and started ...  

>The issue surfaced quickly: i have to copy several lines of code from my laptop in the office  
and of course i cannot cc/paste from rdesktop...   
since I want to make github my systematic doc/wiki, i had the idea of creating the doc repo on github and clone it from office in vscode    

>clone is simple: *git clone https://github.com/steve3535/thelinuxlabs.git*  

first trap here is the security policy in the office: cannot go on the internet directly   
I have to use my JUmp server and set the proxy in my terminal and decide to run all git commands in the terminal (still within vscode)  
Previous to that, I made sure I have the explorer opened thru remote ssh to my Jump  

second issue: how to push ? (because for some obscure reason i didnt want to use ssh keys) but rather https tokens   
(and its actually the only choice because the proxy works for http/https not TCP(ssh) !!!   

solution was to leverage on personal token to be used on the cli with the github client  

1. install the github client  
   `dnf config-manager --add-repo https://cli.github.com/packages/rpm/gh-cli.repo`  
   `dnf install gh`  
   (on ubuntu: ** apt installl gh**)  
2. `gh auth login`
3. Note tht the above command will cache the credentials as well  
4. 


