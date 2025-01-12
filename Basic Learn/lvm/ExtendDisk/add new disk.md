اگر درخواست افزایش هارد داشته باشیم و یک دیسک جدید به ما داده باشن مثل dev/sdc باید از دستورات زیر استفاده کرد:


```
lsblk
```
find new disk :

/dev/sdc

```
vgs
```
find volume group:

vg_os
```
vgextend vg_os /dev/sdc
```
```
lvs
df -h
```
```
lvextend -l +100%FREE -r /dev/mapper/vg_os-var

df-h
```
done
