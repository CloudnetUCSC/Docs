# SEREVER CONFIGURATIONS

## IP Details
- Gateway : `10.22.196.254`
- DNS : `10.22.254.10`
- VMs : `10.22.196.6 - 10.22.196.122 & 10.22.196.126 - 10.22.196.253`

## Server Details
- cloudnet-s01
    - Type : IBM
    - Location : NOC
    - IP : `10.22.196.1`
    - Role : NFS Server | Source or Destination
- cloudnet-s02
    - Type : IBM
    - Location : NOC
    - IP : `10.22.196.2`
    - Role : Source or Destination
- cloudnet-s03
    - Type : IBM
    - Location : NOC
    - IP : `10.22.196.3`
    - Role : Source or Destination
- cloudnet-s04
    - Type : IBM
    - Location : NOC
    - IP : `10.22.196.4`
    - Role : Source or Destination
- cloudnet-s05
    - Type : IBM
    - Location : NOC
    - IP : `10.22.196.5`
    - Role : Source or Destination
- cloudnet1
    - Type : 
    - Location : Research Lab
    - IP : `10.22.196.123`
    - Role : NFS Server | Destination
- cloudnet2
    - Type : 
    - Location : Research Lab
    - IP : `10.22.196.124`
    - Role : Source

`cloudnet-s01 to cloudnet-s05 servers are physically placed bottom to top in NOC`

## Network File System

### Setting Up NFS Server

- Follow below steps to setup the NFS server.

```bash
    sudo su
    apt update
    apt install nfs-kernel-server
    mkdir -p /mnt/nfs
    chown -R nobody:nogroup /mnt/nfs/
    chmod 777 /mnt/nfs/
    nano /etc/exports
    --> /mnt/nfs <start_ip_range>/24(rw,sync,no_subtree_check) | Add this line to /etc/exports
    exportfs -a
    systemctl restart nfs-kernel-server
    ufw disable 
```
- Already cloudnet-s01 is configured as `NFS Server` and the NFS directory is `\mnt\nfs\`.

### Setting Up NFS Client

- Follow below steps to mount NFS server in client

```bash
    sudo su
    apt update
    apt install nfs-common
    mkdir -p /mnt/nfs
    ufw disable 
    mount 10.22.196.1:/mnt/nfs/ /mnt/nfs/
```

- The NFS directory is mounted to `cloudnet-s02 - cloudnet-s05` at the startup. `Therefore no need to mount it again!!!`

- `cloudnet1` server is also configured as a `NFS Server` setuped in `/var/lib/libvirt/images/` directory

## Configuring Netork in Host Machines

- Run below code to setup a bridge, assign ip to the bridge and add gateway 

    ```bash
    ip link add dev br0 type bridge
    ip link set dev br0 up
    ip link set dev enp1s0 up
    ip link set dev enp1s0 master br0
    ip addr add <host_ip>/24 dev br0
    ip route add default via 10.22.196.254 dev br0
    ```
    - This is already setuped in both host machines, if the bridge in a machine is removed accidently use the above code to set it up again.