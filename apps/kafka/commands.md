# دستورات پرکاربرد Kafka

یکی از محبوب‌ترین پلتفرم‌های پردازش جریان داده است. در ادامه، پرکاربردترین دستورات Kafka همراه با توضیح ساده آورده شده است:

---

## 1. **ایجاد یک تاپیک (Topic)**
```bash
kafka-topics.sh --create --topic <topic-name> --bootstrap-server <broker-address>
```
**توضیح:**  
یک تاپیک جدید در Kafka ایجاد می‌کند. تاپیک مثل یک کانال است که پیام‌ها در آن ذخیره می‌شوند.

---

## 2. **مشاهده لیست تاپیک‌ها**
```bash
kafka-topics.sh --list --bootstrap-server <broker-address>
```
**توضیح:**  
لیست همه تاپیک‌هایی که روی Kafka ساخته شده‌اند را نشان می‌دهد.

---

## 3. **توضیحات تاپیک (Describe Topic)**
```bash
kafka-topics.sh --describe --topic <topic-name> --bootstrap-server <broker-address>
```
**توضیح:**  
اطلاعات مربوط به یک تاپیک خاص، مثل تعداد پارتیشن‌ها، وضعیت replication و leader را نشان می‌دهد.

---

## 4. **ارسال پیام به یک تاپیک (Producer)**
```bash
kafka-console-producer.sh --topic <topic-name> --bootstrap-server <broker-address>
```
**توضیح:**  
به شما اجازه می‌دهد تا پیام‌ها را به صورت دستی به یک تاپیک ارسال کنید.

---

## 5. **خواندن پیام‌ها از یک تاپیک (Consumer)**
```bash
kafka-console-consumer.sh --topic <topic-name> --from-beginning --bootstrap-server <broker-address>
```
**توضیح:**  
پیام‌های موجود در یک تاپیک را می‌خواند. گزینه `--from-beginning` برای خواندن تمام پیام‌های گذشته است.

---

## 6. **حذف یک تاپیک**
```bash
kafka-topics.sh --delete --topic <topic-name> --bootstrap-server <broker-address>
```
**توضیح:**  
یک تاپیک و تمام پیام‌های داخل آن را حذف می‌کند. (در صورتی که تنظیمات کافکا اجازه حذف بدهند).

---

## 7. **مانیتور وضعیت مصرف‌کننده‌ها (Consumer Groups)**
```bash
kafka-consumer-groups.sh --describe --group <group-name> --bootstrap-server <broker-address>
```
**توضیح:**  
وضعیت گروه‌های مصرف‌کننده را نشان می‌دهد، مثل offset فعلی و lag (تفاوت بین پیام‌های موجود و پیام‌های خوانده‌شده).

---

## 8. **مشاهده لیست گروه‌های مصرف‌کننده**
```bash
kafka-consumer-groups.sh --list --bootstrap-server <broker-address>
```
**توضیح:**  
لیست گروه‌های مصرف‌کننده موجود در Kafka را نمایش می‌دهد.

---

## 9. **بازنشانی offset ها (Reset Offsets)**
```bash
kafka-consumer-groups.sh --reset-offsets --group <group-name> --topic <topic-name> --to-earliest --bootstrap-server <broker-address> --execute
```
**توضیح:**  
آفست مصرف‌کننده‌ها را تنظیم مجدد می‌کند. این کار برای مدیریت lag یا بازپخش پیام‌ها مفید است.

---

## 10. **بررسی سلامت Kafka Broker**
```bash
kafka-broker-api-versions.sh --bootstrap-server <broker-address>
```
**توضیح:**  
نسخه API و پروتکل‌هایی که Kafka broker پشتیبانی می‌کند را نشان می‌دهد. برای بررسی سلامت و سازگاری سیستم مفید است.

---

## 11. **مشاهده پیام‌ها با یک فیلتر خاص**
```bash
kafka-console-consumer.sh --topic <topic-name> --bootstrap-server <broker-address> --property print.key=true --property key.separator="|"
```
**توضیح:**  
پیام‌ها را با نمایش کلید و مقدار آن‌ها می‌خواند. مناسب برای بررسی پیام‌های Key-Value.

---

## 12. **بررسی Latency تاپیک**
```bash
kafka-run-class.sh kafka.tools.GetOffsetShell --topic <topic-name> --bootstrap-server <broker-address>
```
**توضیح:**  
آخرین offset هر پارتیشن را نشان می‌دهد که برای بررسی latency مفید است.

---

## 13. **ایجاد یک تولیدکننده و مصرف‌کننده به صورت تست**
```bash
kafka-producer-perf-test.sh --topic <topic-name> --num-records 100 --record-size 100 --throughput 1000 --bootstrap-server <broker-address>
kafka-consumer-perf-test.sh --topic <topic-name> --messages 100 --bootstrap-server <broker-address>
```
**توضیح:**  
عملکرد Kafka را با ارسال و مصرف پیام‌های تست اندازه‌گیری می‌کند.

---

## نکته:  
- ابزارها در پوشه `bin` کافکا قرار دارند.
- 
- حتماً مسیر `JAVA_HOME` تنظیم شده باشد.
- 
- آدرس `bootstrap-server` معمولاً به صورت `<IP>:9092` است.
- 
