create 3 interface network for vm
set hostname for all vms
disable selinux and firewalld
yum install libstoragemgmt
install and config chronyd
rm -f /etc/machine-id
systemd-machine-id-setup

add repo ceph
[ceph]
name=Ceph packages for $basearch
baseurl=https://download.ceph.com/rpm-18.2.2/el8/$basearch
enabled=1
priority=2
gpgcheck=1
gpgkey=https://download.ceph.com/keys/release.asc

[ceph-noarch]
name=Ceph noarch packages
baseurl=https://download.ceph.com/rpm-18.2.2/el8/noarch
enabled=1
priority=2
gpgcheck=1
gpgkey=https://download.ceph.com/keys/release.asc

[ceph-source]
name=Ceph source packages
baseurl=https://download.ceph.com/rpm-18.2.2/el8/SRPMS
enabled=0
priority=2
gpgcheck=1
gpgkey=https://download.ceph.com/keys/release.asc

#on all node
yum install cephadm podman ceph-common



#on all node
#vim /etc/containers/registries.conf
[[registry]]
location="repo.test.co:7049"
insecure=true


#only on first and second nodes login
podman login repo.test.co:7049

#then on all node (add prefix="quay.io)
#vim /etc/containers/registries.conf
[[registry]]
prefix="quay.io
location="repo.test.co:7049"
insecure=true


#pull all images and push to nexus
example: podman pull quay.io/ceph/ceph:v19.1.0

podman images
REPOSITORY                        TAG         IMAGE ID      CREATED        SIZE
quay.io/ceph/ceph                 v18.2.2     3c937764e6f5  2 months ago   1.28 GB
quay.io/ceph/ceph-grafana         9.4.7       954c08fa6188  7 months ago   647 MB
quay.io/prometheus/prometheus     v2.43.0     a07b618ecd1d  16 months ago  235 MB
quay.io/prometheus/alertmanager   v0.25.0     c8568f914cd2  19 months ago  66.5 MB
quay.io/prometheus/node-exporter  v1.5.0      0da6a335fe13  20 months ago  23.9 MB

on node1
---------
cephadm --image  quay.io/ceph/ceph:v19.1.0  bootstrap --mon-ip 192.168.114.54  --cluster-network 20.20.20.0/24  --allow-fqdn-hostname


Ceph Dashboard is now available at:

             URL: https://ceph1.test.co:8443/
            User: admin
        Password: tdm3kdans2

Enabling client.admin keyring and conf on hosts with "admin" label
Saving cluster configuration to /var/lib/ceph/05d96de8-4dbe-11ef-8e5e-0050569c450b/config directory
Enabling autotune for osd_memory_target
You can access the Ceph CLI as following in case of multi-cluster or non-default config:

        sudo /usr/sbin/cephadm shell --fsid 05d96de8-4dbe-11ef-8e5e-0050569c450b -c /etc/ceph/ceph.conf -k /etc/ceph/ceph.client.admin.keyring

Or, if you are only running a single cluster on this host:

        sudo /usr/sbin/cephadm shell

Please consider enabling telemetry to help improve Ceph:

        ceph telemetry on

For more information see:

        https://docs.ceph.com/en/latest/mgr/telemetry/





ceph cephadm get-pub-key > ~/ceph.pub
ceph cephadm get-ssh-config > ~/ssh_config
ceph config-key get mgr/cephadm/ssh_identity_key > ~/cephadm_private_key
chmod 0600 ~/cephadm_private_key

ssh-copy-id -f -i ~/ceph.pub root@ceph2.test.co
ssh-copy-id -f -i ~/ceph.pub root@ceph3.test.co
ssh -F ~/ssh_config -i ~/cephadm_private_key root@ceph2.test.co

fron dashvord or cli :
ceph orch host add ceph2.test.co
ceph orch host add ceph3.test.co

from dashbord
---------------
>create osds
ceph orch apply osd --all-available-devices

if failed
vvvvvvvvvv
chephadm shell
cd /var/lib/ceph/bootstrap-osd
touch ceph.keyring
chmod 700 /var/lib/ceph/bootstrap-osd/ceph.keyring
chown -R ceph:ceph bootstrap-osd/
ceph auth get-or-create client.bootstrap-osd mon 'allow profile bootstrap-osd' -o /var/lib/ceph/bootstrap-osd/ceph.keyring
ceph-volume lvm prepare --data /dev/sdb
ceph-volume lvm activate 0 aab25dbd-8cf7-4482-8c6a-70b5a252299c
if failed
ceph orch apply osd --all-available-devices



>create pool (cephfs)
pooltype = replicated
PG Autoscale=off
Placement groups=128
Replicated size=3
Applications=cephfs
create Crush ruleset : chr1


>create pool (cephfs-metadata)
pooltype = replicated
PG Autoscale=off
Placement groups=64
Replicated size=3
Applications=cephfs
create Crush ruleset : chr1


#ceph fs new <fs_name> <metadata> <data>
$ceph fs new cephfs cephfs-metadata cephfs



ceph orch apply mds cephfs --placement="2 ceph1.test.co ceph2.test.co"

if error : MDS_INSUFFICIENT_STANDBY 
$ceph fs set cephfs max_mds 2






*************commands****************
####after reboot enter this command 
ceph orch host rescan ceph2.test.co
ceph orch host ls
ceph status 
ceph host ls 
ceph osd ls


