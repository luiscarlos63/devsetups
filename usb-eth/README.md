# This is a tutorial on how to use usb to create a virtual ethernet interface

## Requirements
### Board/Server side
- Kernel suport for USB Ethernet. Make sure the kernel is build with
```
CONFIG_USB_GADGET=y
CONFIG_USB_ETH=y
CONFIG_USB_MUSB_HDRC=y
CONFIG_USB_MUSB_AM335X=y
```
  Given this, you should be able to see something like **usb0:** interface on `ifconfig` output
- DHCP (ex: udhcpd)


##Quick guide for automatic Host side interface creation
1. On server side, create  **/etc/systemd/network/usb0.network**. You can copy the file provided here
2. Then:
```
   $ systemctl enable systemd-networkd
   $ systemctl restart systemd-networkd
```
You can confirme that the **usb0:** has the IP. 
```
   $ ifconfig usb0
```

3. On server side, create **/etc/udhcpd.conf**. You can copy the file provided here
2. Then:
```
    $ udhcpd -f -S /etc/udhcpd.conf &
    usb0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
    inet 192.168.7.2  netmask 255.255.255.0  broadcast 192.168.7.255

```

From this point on, you should be able to see the Host side interface and the it's IP:

```
    $ ifconfig <enxXXXXXXXX>
    <enxXXXXXXXX>: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
    inet 192.168.7.1  netmask 255.255.255.0  broadcast 192.168.7.255

```


