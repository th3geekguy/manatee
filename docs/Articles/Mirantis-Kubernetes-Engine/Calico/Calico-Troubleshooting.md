---
tags:
  - networking
  - MKE
  - Calico
  - protocol
---

# Calico Troubleshooting

- Calico is a CNI provider for Kubernetes
  - can enforce network policy
- creates daemonset -- and correspondingly a container on each node -- that enforces network policy by either:
  1. iptables
  2. Windows HNS
  3. eBPF
- Felix is the component that handles the network policy (written in goLang)
  - does routing when using VXLAN
- On-prem environments also run BIRD -- which is a BGP routing daemon
  - is used for IP in IP or native networking

## What is Calico Node (Felix)?

- It's a handful of tools (arp, contrack, iproute2, netfilter, procps, kmod) that orchestrate networking on the Kubernetes node

## How do I even look at this stuff (Felix)?

- Logging (Environment Variables)
  - logFilePath: `/var/log/calico/felix.log`
  - logPrefix: calico-packet
  - logSeverityFile | logSeverityScreen | logSeveritySys: Info
    - Levels: Debug, Info, Warning, Error, Fatal
  - flowLogsFileDirectory: `/var/log/calico/flowlogs`
  - dnsCacheFile: `/var/run/calico/felix-dns-cache.txt`
  - dnsLogsFileDirectory: `/var/log/calico/dnsLogs`
  - bpfEnabled: true

## What about BIRD?

- BIRD(6) Logging
    - BIRD(6) logs by default to `/var/log/calico`
    - Appending configuration into `/var/log/calico/bird/config`
        - If asked, each protocol is capable of writing trace messages about its work to the log
        - Protocol debugging options: All | off | trace | route | filters | interfaces | events | packets
        - States
            * protocol is going up, down, starting, stopping, etc.
        - Routes
            * routes exchanged with the routing table
        - Filters
          - details on route filtering
