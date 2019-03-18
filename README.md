# bpf-iptables

## Introduction

`bpf-iptables` is an `eBPF` and `XDP` based firewall, providing same `iptables` syntax.

Thanks to efficient `matching algorithms`, `eBPF` and `XDP` driver level optimizations, is able to provide *high performances*.
No kernel modification are required, `bpf` comes at zero cost with recent Linux kernels.

## Research papers

-
-
-


## How to use?

`bpf-iptables` is part of `PolyCube` framework, so will be called `pcn-iptables` (pcn=PolyCubeNetwork).

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

Refer to Polycube Quickstart for bare metal install mode.
https://github.com/polycube-network/polycube/blob/master/Documentation/quickstart.rst#quick-start


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
A:Yes, `bpf-iptables` is independent.

Q:
A:


### Disclaimer

bpf-iptables is not related to bpfilter (https://lwn.net/Articles/747551/).
Right now bpf-iptables uses a different mechanism to intercept iptables rules.
