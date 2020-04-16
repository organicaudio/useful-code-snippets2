## network stuff

### ssh


```shell
# add -v for verbose mode

# connect
ssh <user>@<ip or url>

# connect on non standard port
ssh -p 12345 <user>@<ip or url>

# connect use rsa private key saved under ~/.ssh/ instead of password
ssh -i ~/.ssh/id_rsa <user>@<ip or url>


# ssh tunnel which connects my local port 1025 with the remote port 4000 over the standard ssh port 22
ssh -L1025:192.168.x.x:4000 -v <user>@<ip or url>
# can be used via
ssh -p1025 <user>@192.168.x.x

```

[nice explanation of ssh tunnels](https://unix.stackexchange.com/questions/115897/whats-ssh-port-forwarding-and-whats-the-difference-between-ssh-local-and-remot)

### check open ports

```shell
# local machine: check for open ports and which process is listening on them
sudo netstat -lptu 

# remote machine: check for open tcp ports (scans the first 1000 ports)
nmap <ip_address>

# remote machine: check for open tcp ports (scans all ports)
nmap -p- <ip_address>

# remote machine: check if tcp port 8080 is open 
nmap -p 8080 <ip_address>
```

with windows powershell:

```
tnc google.com -port 80
```

### open ports (iptables)

```shell
# lists all rules
sudo iptables -L 

# save all rules
iptables-save > ipconfig_rules_backup.txt 

# restore old rules
iptables-restore < ipconfig_rules_backup.txt

# add new rule syntax
# add new rule example
TODO
```
