# Usefull systemd stuff

## Execute service after internet available(dhcpcd)

Most of the time the `After=network-online.target` will activate before your network is configured. <br>
If you are using **dhcpcd** you can create a service specific to your network interface which will wait until your network is fully configured. <br>

Find out your used network using `ip link`. For example:
``` bash
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether *:*:*:*:*:* brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    altname enxec******
3: wlp2s0b1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether *:*:*:*:*:* brd ff:ff:ff:ff:ff:ff
    altname wlxe******
```

I'm using the lan port so my interface is **eno1**. <br>
Disable **dhcpcd** with `sudo systemctl disable dhcpcd.service` which will **only** disable but not stop **dhcpcd**!

Now enable the **dhcpcd** service with your specific interface. <br>
`systemctl enable dhcpcd@<network adapter>.service` <br>

In my case: <br>
`sudo systemctl enable dhcpcd@eno1.service`

Not you can stop the old **dhcpcd** service and start the new one or just restart(when using ssh)

Now you can use the following **Wanted** and **After** targets:
``` bash
After=dhcpcd@<network adapter>.service
Wants=dhcpcd@<network adapter>.service

# For me
After=dhcpcd@eno1.service
Wants=dhcpcd@eno1.service
```
