اگر افزایش حجم برروی یکی از دیسک های موجود باشد باید از دستورات زیر استفاده کرد
در این مثال افزایش حجم برروی sda بوده است

```
cfdisk 
```
Go to the partition you wish to increase the disk size
Select resize
enter the amount of free space to use then write.
write 
quit

```
partprobe /dev/sda3
```
```
pvs
```
```
pvresize /dev/sda3
```
```
vgdisplay
```
```
lvextend -l +100%FREE -r /dev/mapper/rl-var
```
```
xfs_growfs /dev/rl/var
```
