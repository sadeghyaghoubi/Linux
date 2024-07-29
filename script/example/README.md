```script
#!/bin/bash

OUTPUT="/home/ubuntu/scripts/script1_`date +%Y%m%d%H%M`.log"
read -p "please enter your name: " myname
read -p "please enter your lastname:" lastname

echo "$myname $lastname" >> $OUTPUT
echo "your name is $myname $lastname"
```
اجرا 

./script


نتیجه:

please enter your name: sadegh

please enter your lastname: yaghoubi

your name is sadegh yaghoubi


 یک فایلی هم ایجاد شده است که تو اسمش تاریخم هست و این اطلاعات بهش اضافه شده .
اگر یکبار دیگه هم بزنم میره زیر همین اضافه میکنه چون از << استفاده کردیم

میشه از بک تیک هم استفاده کرد `date +%Y%m%d%H%M`
میشه hostname هم تو اسم بذاریم `hostname`
 

