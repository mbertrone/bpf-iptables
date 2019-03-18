# bpf-iptables

## Introduction

`bpf-iptables` is an `eBPF` and `XDP` based firewall, providing same `iptables` syntax.

Thanks to efficient `matching algorithms`, `eBPF` and `XDP` driver level optimizations, is able to provide *high performances*.
No kernel modification are required, `bpf` comes at zero cost with recent Linux kernels.

## Research papers

### Securing Linux with a Faster and Scalable Iptables
*Draft, 1 December 2018*  
This paper presents an eBPF-based firewall, bpf-iptables, which emulates the iptables filtering semantic while guaranteeing higher throughput outperforming other Linux-based firewalls particularly when a high number of rules is involved.
[PDF](https://mbertrone.github.io/documents/21-Securing_Linux_with_a_Faster_and_Scalable_Iptables.pdf)

### Accelerating Linux Security with eBPF iptables
*ACM SIGCOMM 2018 Conference Posters and Demos, Budapest (H), 20-25 August 2018*  
This paper presents an eBPF-based prototype that emulates the iptables filtering semantic and exploits a more efficient matching algorithm, without requiring custom kernels or invasive software frameworks.
[PDF](https://mbertrone.github.io/documents/19-eBPF-Iptables-Demo.pdf)

### Toward an eBPF-based clone of iptables
*Netdev 0x12, The Technical Conference on Linux Networking, Montréal (Canada), 11-13 July 2018*  
This paper reports the first results of a project that aims at creating a eBPF-based (partial) clone of iptables. This project assumes unmodified Linux kernel and guarantees the full compatibility with current iptables.
[PDF](https://mbertrone.github.io/documents/20-eBPF-Iptables-Netdev.pdf)

## How to use?

`bpf-iptables` is part of `PolyCube` framework. We use `pcn-iptables` syntax (`pcn=PolyCubeNetwork`).

### Docker

```
# Pull docker image (PolyCube & pcn-iptables)
docker pull polycubenetwork/polycube:latest

# Run the Polycube Docker and launch polycubed (the polycube daemon) inside it.
# The Docker container is launched in the host networking stack (--network host),
# privileged mode (--privileged) is necessary to use eBPF features.
docker run  -it --rm --privileged --network host \
-v /lib/modules:/lib/modules:ro -v /usr/src:/usr/src:ro -v /etc/localtime:/etc/localtime:ro \
polycubenetwork/polycube:latest /bin/bash -c 'polycubed -d && /bin/bash'

```

Refer to Polycube Quickstart for bare metal install mode. [Quickstart](https://github.com/polycube-network/polycube/blob/master/Documentation/quickstart.rst#quick-start)


```
# Initialize pcn-iptables
pcn-iptables-init
```

```
# pcn-iptables provides same iptables syntax. Please ref#er to iptables online docs for more info.
# Following are just few examples of available commands.

# E.g.
pcn-iptables -A INPUT -s 10.0.0.1 -j DROP # Append rule to INPUT chain
pcn-iptables -D INPUT -s 10.0.0.1 -j DROP # Delete rule from INPUT chain
pcn-iptables -I INPUT -s 10.0.0.2 -j DROP # Insert rule into INPUT chain

# Example of a complex rule
pcn-iptables -A INPUT -s 10.0.0.0/8 -d 10.0.0.2 -p tcp --sport 9090 --dport 80 --tcpflags SYN,ACK ACK -j DROP

# Example of a conntrack rule
pcn-iptables -A OUTPUT -m conntrack --ctstate=ESTABLISHED -j ACCEPT

# Show rules
pcn-iptables -S # dump rules
pcn-iptables -L INPUT # dump rules for INPUT chain

pcn-iptables -P FORWARD DROP # set default policy for FORWARD chain

```

```
# Stop and clean pcn-iptables
pcn-iptables-clean
```

## Q&A
Q:Can I still use `iptables`?  
A:Yes, iptables will not be affected.

Q:Advantages?  
A:Performance (especially with a large amount of rules); Low CPU utilization (especially with XDP mode)

Q:How to use XDP mode?  
A:run `pcn-iptables-init-xdp`

Q:Limitations of XDP mode?  
A:`pcn-itpables` will be atached only to XDP compatible interfaces.


## Links
[pcn-iptables Source Code](https://github.com/polycube-network/polycube/tree/master/src/services/pcn-iptables)  
[pcn-iptables Documentation](https://github.com/polycube-network/polycube/blob/master/Documentation/components/iptables/pcn-iptables.rst)  
[PolyCube Network](https://github.com/polycube-network/polycube)


### Disclaimer

bpf-iptables is not related to bpfilter (https://lwn.net/Articles/747551/).  
Right now bpf-iptables uses a different mechanism to intercept iptables rules.
