# vi yaml config
```
vi .vimrc
syntax on
autocmd FileType yaml setlocal ts=2 sts=2 sw=2 expandtab autoindent
```

# NAT
```
sudo iptables -t nat -A PREROUTING -i [firewall-public-interface] -p tcp --dport 80 -j DNAT --to-destination [web-server-private-ip]

sudo iptables -t nat -A POSTROUTING -o [firewall-private-interface] -p tcp --dport 80 -d [web-server-private-ip] -j SNAT --to-source [firewall-private-ip]
```

```
sudo iptables -t nat -A PREROUTING -i enp2s0 -p tcp --dport 20001 -j DNAT --to-destination 192.168.56.204:20001

sudo iptables -t nat -A POSTROUTING -o vboxnet0 -p tcp --dport 20001 -d 192.168.56.204:20001 -j SNAT --to-source 192.168.56.1
```