# مراحل نصب Kafka

### پیش‌نیازها

- از **Stop** و **Disable** بودن **Firewall** اطمینان حاصل کنید.
- **SELinux** را در حالت **Permissive** قرار دهید.

### ایجاد پارتیشن برای دیسک دیتا (اختیاری)

در صورتی که برای دیتا سرویس مورد نظر از دیسک مجزا استفاده شود، باید یک پارتیشن روی دیوایس مورد نظر ایجاد کنید. مراحل ایجاد پارتیشن به شرح ذیل است:

#### a. ساخت پارتیشن برای دیسک دیتا

```bash
# fdisk <device-name>
```
به عنوان مثال:

```bash
[root@localhost ~]# fdisk /dev/sdc

Welcome to fdisk (util-linux 2.32.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x9379b641.

Command (m for help): g
Created a new GPT disklabel (GUID: 83B4445D-DE99-3B4A-8870-C9A4C73AAFF4).

Command (m for help): n
Partition number (1-128, default 1):
First sector (2048-20971486, default 2048):
Last sector, +sectors or +size{K,M,G,T,P} (2048-20971486, default 20971486):

Created a new partition 1 of type 'Linux filesystem' and of size 10 GiB.

Command (m for help): p
Disk /dev/sdc: 10 GiB, 10737418240 bytes, 20971520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 83B4445D-DE99-3B4A-8870-C9A4C73AAFF4

Device     Start      End  Sectors Size Type
/dev/sdc1   2048 20971486 20969439  10G Linux filesystem

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

#### b. ساختن فایل سیستم

```bash
# mkfs.xfs /dev/sdc1
```

#### c. ایجاد دایرکتوری دیتا و مانت کردن دیسک دیتا به آن

```bash
# mkdir /data 
# mount /dev/sdc1 /data
```

#### d. پرسیست کردن دیسک

```bash
# vim /etc/fstab
/dev/sdc1  /data xfs defaults 0 0
```

#### e. ایجاد دایرکتوری کافکا

```bash
# mkdir /data/kafka
# chown –R cp-kafka:confluent /data/kafka
```

---

## 2. نصب جاوا

```bash
# yum install -y java-11-openjdk
```

---

## 3. نصب سرویس

### a. ایمپورت کردن gpg-key

```bash
# rpm –import https://packages.confluent.io/rpm/5.5/archive.key
```

### b. اضافه کردن ریپو کانفلوئنت

```bash
vim /etc/yum.repos.d/confluent.repo
```

محتوای فایل:

```
[Confluent.dist]
name=Confluent repository (dist)
baseurl=https://packages.confluent.io/rpm/5.5/8
gpgcheck=1
gpgkey=https://packages.confluent.io/rpm/5.5/archive.key
enabled=1

[Confluent]
name=Confluent repository
baseurl=https://packages.confluent.io/rpm/5.5
gpgcheck=1
gpgkey=https://packages.confluent.io/rpm/5.5/archive.key
enabled=1
```

### c. نصب سرویس کافکا

```bash
# yum update –y && yum install -y confluent-kafka
```

---

## 4. کانفیگ سرویس

### حالت کلاستر

#### a. ایجاد فایل myid

برای هر سرور یک فایل `myid` در مسیر دیتا دایرکتوری **Zookeeper** ایجاد کنید:

```bash
# In the srv1 server
echo "1" > /var/lib/zookeeper/myid
# In the srv2 server
echo "2" > /var/lib/zookeeper/myid
# In the srv3 server
echo "3" > /var/lib/zookeeper/myid
```

#### b. کانفیگ zookeeper

فایل کانفیگ سرویس Zookeeper:

```bash
# vim /etc/kafka/zookeeper.properties
```

محتوا:

```
maxClientCnxns=60
admin.enableServer=true
admin.serverPort=8080
tickTime=2000
initLimit=10
syncLimit=5
server.0=kafka1.local:2888:3888
server.1=kafka2.local:2888:3888
server.2=kafka3.local:2888:3888
```

#### c. کانفیگ kafka

فایل کانفیگ سرویس Kafka:

```bash
# vim /etc/kafka/server.properties
```

محتوا:

```
broker.id=0
advertised.listeners=PLAINTEXT://kafka1.local:9092
log.dirs=/data/kafka
offsets.topic.replication.factor=3
transaction.state.log.replication.factor=3
transaction.state.log.min.isr=2
zookeeper.connect=kafka1.local:2181,kafka2.local:2181,kafka3.local:2181
```

#### d. استارت کردن سرویس‌ها

```bash
# systemctl enable --now confluent-zookeeper.service
# systemctl enable --now confluent-kafka.service
```

#### e. ایجاد تاپیک‌ها

```bash
# kafka-topics –bootstrap-server 127.0.0.1:9092 --create --topic swift-bfile --replication-factor 3 --partition 6
# kafka-topics –bootstrap-server 127.0.0.1:9092 --create --topic search-bfile --replication-factor 1 --partition 6
```

---

### حالت تک نود

#### a. کانفیگ Zookeeper

در حالت تک نود نیازی به کانفیگ Zookeeper نیست.

#### b. کانفیگ Kafka

```bash
# vim /etc/kafka/server.properties
```

محتوا:

```
log.dirs=/data/kafka
```

#### c. استارت کردن سرویس‌ها

```bash
# systemctl enable --now confluent-zookeeper.service
# systemctl enable --now confluent-kafka.service
```

#### d. ایجاد تاپیک‌ها

```bash
# kafka-topics –bootstrap-server 127.0.0.1:9092 --create --topic swift-bfile --replication-factor 1 --partition 1
# kafka-topics –bootstrap-server 127.0.0.1:9092 --create --topic search-bfile --replication-factor 1 --partition 1
