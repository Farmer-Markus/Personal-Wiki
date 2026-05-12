# Raspberry pi os

## Remove **_.local_** from hostname/network alias

Raspberry pi os seems to use **[avahi](https://avahi.org/)** to make itself availably using the **_.local_** ending. <br>
But we want just the hostname without the **_.local_**!

### Using **NetworkManager** (tested)

Open the NetworkManager hostname config:
``` bash
sudo nano /etc/NetworkManager/conf.d/hostname.conf
```

Add the following and set the hostname you want to use:
``` ini
[Connection]
hostname=<yourhostname>
```

Then restart NetworkManager
``` bash
sudo systemctl restart NetworkManager
```

### Using **dhcpcd** (untested pls report in an issue if worked/or not)

If you are using an older os you might need to change the **_dhcpcd_** configuration. <br>
Open the config:
``` bash
sudo nano /etc/dhcpcd.conf
```

Search for a `hostname` entry, remove the `#` at the start of the line and **reboot** the pi.
``` bash
sudo reboot
```
<br>
<br>
Done, now your pi should be available with your custom hostname without using **_.local_**


### Disabling **avahi**

Now we can disable the **_avahi_** daemon:
``` bash
sudo systemctl stop avahi-daemon
sudo systemctl disable avahi-daemon
```
