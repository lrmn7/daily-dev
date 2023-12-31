# Using another computer as a gateway

**Last updated**: July 1, 2020

<center>
```mermaid
graph LR
WAN[WAN] -->|eth0| A[Node A]
A -->|eth1| B[Node B]
```
</center>

- `eth0`: WAN (public) (e.g., `enp0s25`)
- `eth1`: LAN (private) (e.g., `bond0`)

**Setup for gateway node:**

```bash
$ iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE # enable NAT on eth0
$ iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT # allow packets from eth1 to go out of eth0
$ iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT # sent the packets to eth1
```

**On LAN nodes:**

```bash
route add default gw <ip_of_gateway>
```
