
# wireless

```
apt-get install wireless-tools usbutils
```
## install hostapd
```
apt-get install hostapd
sudo apt-get remove hostapd
```

download RTL8188C

```
https://github.com/wxjeacen/RTL8188C
```

```
cd wpa_supplicant_hostapd/wpa_supplicant_hostapd-0.8/hostapd
make clean
make
make install
sudo cp /usr/local/bin/* /usr/sbin/
```
```
cd /home/pi/RTL8188C/wireless_tools/
tar zxvf  wireless_tools.30.rtl.tar.gz
cd wireless_tools.30.rtl
make clean
make
make install
```
### vim /etc/hostapd/hostapd.conf
```
nterface=wlan0
driver=rtl871xdrv
ssid=pi
hw_mode=g
channel=3
wpa=2 (WPA2加密ONLY， WIFI會跟我一樣瞬間斷訊號的記得選2)
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP CCMP
rsn_pairwise=CCMP
wpa_passphrase=1234567890 
```
#### autostart hostapd
```
vim /etc/default/hostapd

RUN_DAEMON=”yes”
DAEMON_CONF=”/etc/hostapd/hostapd.conf”
```

### vim /etc/network/interfaces
```
auto wlan0
iface wlan0 inet static
address 192.168.1.1 
netmask 255.255.255.0
```
## DNS
```
apt-get install dnsmasq
vim /etc/dnsmasq.conf 

interface=wlan0
dhcp-range=192.168.1.2,192.168.1.127,12h
```
### autostart
```
vim /etc/default/dnsmasq

DNSMASQ_OPTS="--conf-file=/etc/dnsmasq.conf"
#CONFIG_DIR=/etc/dnsmasq.d
```
```
/etc/init.d/dnsmasq restart
```
```
apt-get install iptables-persistent
```
```
sysctl net.ipv4.ip_forward=1
iptables -A INPUT -i wlan0 -j ACCEPT
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j MASQUERADE
```