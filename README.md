# ssvpn
Secure Shell Vpn (debian)

Upstart script to create a vpn using ssh.

Instructions
- Place ssvpn script in /etc/init.d/ssvpn
- Create configuration in /etc/default/ssvpn
- Create .ssh/config Host entries to configure sessions 

Example configuration:

Server101: /etc/default/ssvpn
```
#!/bin/sh

BRIDGE="br0"
ADDRESS="192.168.111.101/24"
# Ifaces that will be created by remote ssh client
IFACES="tap201 tap202"
```

Server102: /etc/default/ssvpn
```
#!/bin/sh

BRIDGE="br0"
ADDRESS="192.168.111.102/24"
# Ifaces that will be created by remote ssh client
IFACES="tap201 tap202"
```

Client201: /etc/default/ssvpn
```
#!/bin/sh

BRIDGE="br0"
ADDRESS="192.168.111.201/24"
# Ifaces that will be created by local ssh client
IFACES="tap101 tap102"
# Ssh Servers that I will connect to
HOSTS="Server101 Server102"
```

Client201: Add to ~/.ssh/config or /etc/ssh/ssh_config
```
Host Server101
        Hostname realname.example.com
        ForwardAgent yes
        ServerAliveCountMax 3
        ServerAliveInterval 60
        PasswordAuthentication no
        PreferredAuthentications publickey
        ExitOnForwardFailure yes
        Tunnel ethernet
        # local iface tap101 remote iface tap201
        TunnelDevice 101:201
    
Host Server102
        Hostname realname.example.com
        ForwardAgent yes
        ServerAliveCountMax 3
        ServerAliveInterval 60
        PasswordAuthentication no
        PreferredAuthentications publickey
        ExitOnForwardFailure yes
        # local iface tap102 remote iface tap202
        Tunnel ethernet
        TunnelDevice 102:201
```

Client202: /etc/default/ssvpn
```
#!/bin/sh

BRIDGE="br0"
ADDRESS="192.168.111.202/24"
# Ifaces that will be created by local ssh client
IFACES="tap101 tap102"
# Ssh Servers that I will connect to
HOSTS="Server101 Server102"
```

Client201: Add to ~/.ssh/config or /etc/ssh/ssh_config
```
Host Server101
        Hostname realname.example.com
        ForwardAgent yes
        ServerAliveCountMax 3
        ServerAliveInterval 60
        PasswordAuthentication no
        PreferredAuthentications publickey
        ExitOnForwardFailure yes
        Tunnel ethernet
        # local iface tap101 remote iface tap202
        TunnelDevice 101:202
    
Host Server102
        Hostname realname.example.com
        ForwardAgent yes
        ServerAliveCountMax 3
        ServerAliveInterval 60
        PasswordAuthentication no
        PreferredAuthentications publickey
        ExitOnForwardFailure yes
        Tunnel ethernet
        # local iface tap102 remote iface tap202
        TunnelDevice 102:202
```
