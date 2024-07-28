# تنظیم IP و Gateway در لینوکس

## تنظیم موقت با استفاده از `ip` و `route`

### تنظیم IP Address
برای تنظیم آدرس IP به صورت موقت، از دستور `ip` استفاده می‌کنید:

```bash
sudo ip addr add 192.168.1.100/24 dev eth0
```

192.168.1.100: آدرس IP که می‌خواهید تنظیم کنید.

/24: Netmask به صورت CIDR.

eth0: نام رابط شبکه.

### تنظیم Default Gateway

برای تنظیم Gateway پیش‌فرض، از دستور ip route استفاده می‌شود:

```bash
sudo ip route add default via 192.168.1.1
```
192.168.1.1: آدرس Gateway (دروازه پیش‌فرض).

## تنظیم دائم با ویرایش فایل‌های پیکربندی

### فایل‌های پیکربندی در توزیع‌های مختلف

در توزیع‌های مختلف لینوکس، فایل‌های پیکربندی متفاوت هستند:

Debian/Ubuntu: /etc/network/interfaces

RHEL/CentOS/Fedora: /etc/sysconfig/network-scripts/ifcfg-<interface>

Arch Linux: /etc/netctl/



### تنظیمات در Debian/Ubuntu

ویرایش فایل /etc/network/interfaces:

```plaintext
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
```

### تنظیمات در RHEL/CentOS/Fedora

ویرایش فایل /etc/sysconfig/network-scripts/ifcfg-<interface>:

```plaintext
DEVICE=eth0
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
```

### تنظیمات در Arch Linux
در Arch Linux، از netctl استفاده می‌شود:

```plaintext
Description='A basic static ethernet connection'
Interface=eth0
Connection=ethernet
IP=static
Address=('192.168.1.100/24')
Gateway='192.168.1.1'
```

## اعمال تغییرات
```bash
sudo systemctl restart networking  # در Debian/Ubuntu
sudo systemctl restart network    # در RHEL/CentOS/Fedora
```
