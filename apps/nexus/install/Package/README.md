
# نصب و راه‌اندازی Nexus به عنوان سرویس در سیستم

این راهنما مراحل نصب و راه‌اندازی Nexus 3 به عنوان یک سرویس در سیستم را توضیح می‌دهد.

---

## **1. نصب Java 1.8**
ابتدا اطمینان حاصل کنید که Java 1.8 روی سیستم شما نصب شده است.
```
yum install java-17-openjdk -y
```
```
mkdir /app && cd /app
```
---

## **2. دانلود و استخراج Nexus**
1. دانلود آخرین نسخه Nexus:
   ```bash
   sudo wget -O nexus.tar.gz https://download.sonatype.com/nexus/3/latest-unix.tar.gz
   ```
2. استخراج فایل فشرده:
   ```bash
   sudo tar -xvf nexus.tar.gz
   ```
3. انتقال Nexus به مسیر مناسب:
   ```bash
   sudo mv nexus-3* nexus
   ```

---

## **3. ایجاد کاربر Nexus**
1. ایجاد کاربر جدید به نام `nexus`:
   ```bash
   sudo adduser nexus
   ```
2. تغییر مالکیت دایرکتوری‌های Nexus:
   ```bash
   sudo chown -R nexus:nexus /app/nexus
   sudo chown -R nexus:nexus /app/sonatype-work
   ```

---

## **4. تنظیمات کاربر Nexus**
1. فایل تنظیمات `nexus.rc` را باز کنید:
   ```bash
   sudo vi /app/nexus/bin/nexus.rc
   ```
2. خط مربوط به `run_as_user` را Uncomment کنید و به شکل زیر تغییر دهید:
   ```
   run_as_user="nexus"
   ```

---

## **5. اجرای Nexus به عنوان سرویس سیستم**
1. ایجاد فایل سرویس برای Nexus:
   ```bash
   sudo vi /etc/systemd/system/nexus.service
   ```
2. محتوای زیر را در فایل وارد کنید:
   ```ini
   [Unit]
   Description=nexus service
   After=network.target

   [Service]
   Type=forking
   LimitNOFILE=65536
   User=nexus
   Group=nexus
   ExecStart=/app/nexus/bin/nexus start
   ExecStop=/app/nexus/bin/nexus stop
   Restart=on-abort

   [Install]
   WantedBy=multi-user.target
   ```

3. فایل را ذخیره کرده و خارج شوید.

---

## **6. فعال‌سازی سرویس Nexus**
1. فعال‌سازی سرویس Nexus:
   ```bash
   sudo chkconfig nexus on
   ```
2. شروع سرویس Nexus:
   ```bash
   sudo systemctl start nexus
   ```
3. اطمینان از فعال بودن سرویس:
   ```bash
   sudo systemctl status nexus
   ```

---

## **جمع‌بندی**
پس از انجام مراحل بالا، Nexus به عنوان یک سرویس سیستم اجرا می‌شود و به صورت خودکار با بوت شدن سیستم شروع به کار می‌کند.
