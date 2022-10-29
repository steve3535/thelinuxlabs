### have dates on history log (tested on rhel8)  

insert the lines in **.bashrc profile**:  
```bash
export HISTTIMEFORMAT="%Y-%m-%d %T "
export PROMPT_COMMAND='trap "" 1 2 15; history -a >(tee -a ~/.bash_history | while read line; do if [[ $line =~ ^#[0-9]*$ ]]; then continue; fi; logger -p user.info -t "bash[$$]" "($USER:${SUDO_USER}: $line)"; done); trap 1 2 15;'
```
TODO => put it in /etc/bashrc and bake it into the gold image  
