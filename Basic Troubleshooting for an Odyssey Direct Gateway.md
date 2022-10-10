# Basic Troubleshooting for an Odyssey Direct Gateway

{{TOC}}

## Initial Connection
- Connect to a remote workstation with network acccess to the Odyssey Gateway
- Determine which tool to use for the SSH connection. See [Connection Tools](#Connection%20Tools), puTTY is usually the best choice.

- Gather the following informtion:
    - IP address, see connection notes
    - username and password, see [Usernames and Passwords](#Usernames%20and%20Passwords)

- Make an SSH connection to the gateway by filling in the IP address, selecting "SSH", and clicking "Open"
- If the connection is successful, you'll be asked to accept the host fingerprint (say yes), then enter the username and password.

## Common Tasks

### Disable or Modify Scheduled Tasks
```
cd /etc/cron.d
```

then, edit one or more of the following files:
```
TODO, get the names of the three proxy cron files
```
See [[Useful Tools#vi]] for more information on editing files


### Find OS Version
```
hostnamectl
```

### Find the Device Uptime
```
uptime
```

### Check the Gateway Settings
```
cat /usr/local/cbord-pcs-proxy/vfep.properties
```

### Restart the Proxy Service
CentOS
```
service cbord-pcs-proxy restart
```

Ubuntu
```
systemctl restart cbord-pcs-proxy
```
TODO, check if service is supported in Ubuntu and vice versa

### Reboot the Gateway
CentOS
```
reboot
```

Ubuntu
```
sudo reboot
```

### Check Listening Ports
```
ss -ant
```
See more about `ss` [[Useful Tools#ss - Show Network Information | here]]

### View Logs
#### Direct version < 20.3

View logs:
```
tail -n 200 /usr/local/cbord/pcs-proxy/vfep.log.0
less /usr/local/cbord/pcs-proxy/vfep.log.0
```

View live logging events:
```
tail -f /usr/local/cbord/pcs-proxy/vfep.log.0
```
- `tail -n x` shows the last x lines, where it's 200 in the example. It could be any number
- `less` is a document viewer, allowing you to move around in the file
- See more about [[Useful Tools#less - view file contents | less]] and  [[Useful Tools#tail - View File Contents as It's Updated | tail]].


#### Direct version 20.3+
View Journals (logs)
```
journalctl -u cbord-pcs-prox --since “02:30”  
```

View live logging events:
```
journalctl -f -u cbord-pcs-proxy
```
- There are additional options, see more about `journalctl` [[Useful Tools#journalctl - view logs| here]]

See [[Odyssey Direct and Gateway#Odyssey Gateway Logs]] for more information about gateway logs.

## Connection Tools
### puTTY
- Small, portable, well-known
- [Download](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
	- I download and keep the 32-bit x86 version _without_ the installer (putty.exe). It's more compatible and doesn't require an install.
- Right click to paste (especially helpful for pasting the password)

### Windows Command Line
- Usually only installed in Windows 10 or Server 2016
- ssh \<user\>@\<hostname | IP address\>
- Example: `ssh root@10.0.1.15`


##   Usernames and Passwords
### Configured Gateways
These are the usernames for configured gateways. __Passwords are in remote connection notes, listed as the VFEP password__.

Example:
```
username: root
password: 6zArelatES9#
```

CentOS 6:
```
username: root
```

CentOS 7:
```
username: root
```

Ubuntu:
```
username: cbord_support
```

### Defaults
Until configured, these are the default usernames and passwords for the Odyssey Direct Gateway. 

CentOS 6:
```textfile
root
Pku%2E6#
```

CentOS 7:
```textfile
root
Cb0rdacce55
```

Ubuntu:
```textfile
cbord_support
ubuntu
```
- In Ubuntu, use ctrl-alt-F1,F2, etc to get a shell from a console, GUI would be F7	(not needed if you're connected remotely via [[#SSH]])  
- NOTE: the root user is disabled by default in Ubuntu
