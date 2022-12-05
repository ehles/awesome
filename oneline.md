# Onliners

Inspect network traffic from remote server

```bash
ssh remote 'tcpdump -U -i eth0 -w - not tcp port 22' | wireshark -k -i -
```

---

Mac: pbcopy will allow you to copy the output of a command right into your clipboard. Vice-versa for pbpaste — it will allow you to paste your clipboard right into your terminal.

```bash
# This will copy all the content within a file.
$ cat myfile.txt | pbcopy
# This will output the contents of your clipboard.
$ pbpaste
```

```bash
# Linux version of OSX pbcopy and pbpaste.
alias pbcopy=’xsel — clipboard — input’
alias pbpaste=’xsel — clipboard — output’
```

---
