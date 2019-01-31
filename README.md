# bpf-iptables

## Introduction

`bpf-iptables` is an `eBPF` and `XDP` based firewall, providing same `iptables` syntax.

Thanks to efficient `matching algorithms`, `eBPF` and `XDP` driver level optimizations, is able to provide *high performances*.
No kernel modules or modification are required, bpf comes at zero cost with recent Linux kernels.

### Disclaimer

bpf-iptables is not related to bpfilter (https://lwn.net/Articles/747551/).
They are independent projects, right now bpf-iptables uses a different mechanism to intercept iptables rules.

## How to use?



## Q&A
Q:Can I still use `iptables`?
A:Yes, `bpf-iptables` is independent.

