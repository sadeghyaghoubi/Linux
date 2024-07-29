```script
#!/bin/bash

read -p "please enter your name: " myname
read -p "please enter your lastname:" lastname

echo "$myname $lastname" >> script1.log
echo "your name is $myname $lastname"
```
اجرا :
./script

نتیجه:
please enter your name: sadegh
please enter your lastname: yaghoubi
your name is sadegh yaghoubi

 یک فایلی هم ایجاد شده است که این اطلاعات بهش اضافه شده .
