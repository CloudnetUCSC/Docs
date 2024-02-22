# SEREVER CONFIGURATIONS

## IP Details
- Gateway : ```10.22.196.254```
- DNS : ```10.22.254.10```
- VMs : `10.22.196.6 - 10.22.196.122 & 10.22.196.126 - 10.22.196.253`

## Server Details
- cloudnet-s01
    - Type : IBM
    - Location : NOC
    - IP : ```10.22.196.1```
- cloudnet-s02
    - Type : IBM
    - Location : NOC
    - IP : ```10.22.196.2```
- cloudnet-s03
    - Type : IBM
    - Location : NOC
    - IP : ```10.22.196.3```
- cloudnet-s04
    - Type : IBM
    - Location : NOC
    - IP : ```10.22.196.4```
- cloudnet-s05
    - Type : IBM
    - Location : NOC
    - IP : ```10.22.196.5```
- cloudnet1
    - Type : 
    - Location : Research Lab
    - IP : ```10.22.196.123```
- cloudnet2
    - Type : 
    - Location : Research Lab
    - IP : ```10.22.196.124```

`Servers cloudnet-s01 to cloudnet-s05 are physically placed bottom to top in NOC`

## Network File System

### Setting Up NFS Serever

- cloudnet `/var/lib/libvirt/images/` directory is shared by Source and Destination Machine.


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

## Network File System

- A distributed file system is required to facilitate Live migration, therefor by using a Network File System `/var/lib/libvirt/images/` directory is shared by Source and Destination Machine.

- Run `startNFS` keyword after starting the Source Machine.