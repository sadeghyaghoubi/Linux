بجای اینکه بیایم 2 خط بنویسیم که یک خروجیو تو یک فایل ذخیره کنه و علاوه بر اون تو خط بعدی بنویسیم همونو چاپ کنه
میایم تو یک خط همین کارو میکنیم
این کار به کمک دستور Tee خواهد بود

## echo "your name is $myname $lastname" | tee $OUTPUT

بجای این :
```bash
#!/bin/bash

OUTPUT="/home/ubuntu/scripts/script1_`date +%Y%m%d%H%M`.log"
read -p "please enter your name: " myname
read -p "please enter your lastname:" lastname

echo "$myname $lastname" >> $OUTPUT
echo "your name is $myname $lastname"
```

از این استفاده میکنیم
```bash
#!/bin/bash

OUTPUT="/home/ubuntu/scripts/script1_`date +%Y%m%d%H%M`.log"
read -p "please enter your name: " myname
read -p "please enter your lastname:" lastname

echo "your name is $myname $lastname" | tee $OUTPUT
```
