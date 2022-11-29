# Onliners

Inspect network traffic from remote server

```bash
ssh remote 'tcpdump -U -i eth0 -w - not tcp port 22' | wireshark -k -i -
```
