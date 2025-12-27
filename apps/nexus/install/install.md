you have to download nexus package
for download package you have to go this site :https://help.sonatype.com/en/download.html
and copy link and download with wget in /opt/

```
sudo useradd -r -m -d /opt/nexus -s /sbin/nologin nexus

sudo tar -xvzf latest-unix.tar.gz -C /opt

sudo mv /opt/nexus-3* /opt/nexus

sudo mv /opt/nexus-3* /opt/nexus

sudo chown -R nexus:nexus /opt/nexus

sudo chown -R nexus:nexus /opt/sonatype-work

sudo vi /opt/nexus/bin/nexus.rc
run_as_user="nexus"

sudo vi /opt/nexus/bin/nexus.vmoptions
-Xms1200m
-Xmx1200m
-XX:MaxDirectMemorySize=2g
-XX:+UnlockExperimentalVMOptions
-XX:+UseG1GC
-XX:MaxGCPauseMillis=200


sudo vi /etc/systemd/system/nexus.service

[Unit]
Description=Nexus Repository Manager
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
User=nexus
Group=nexus
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
Restart=on-abort

[Install]
WantedBy=multi-user.target


sudo systemctl daemon-reexec
sudo systemctl daemon-reload

sudo systemctl enable nexus
sudo systemctl start nexus

sudo systemctl status nexus

sudo cat /opt/sonatype-work/nexus3/admin.password

netstat -ntulp
```
