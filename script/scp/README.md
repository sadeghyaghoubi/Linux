#!/bin/bash

OUTPUT="/home/ubuntu/scripts/script1_`date +%Y%m%d%H%M`.log"
read -p "please enter your name: " myname
read -p "please enter your lastname:" lastname

echo "your name is $myname $lastname" | tee $OUTPUT
scp $OUTPUT root#ip:/home
