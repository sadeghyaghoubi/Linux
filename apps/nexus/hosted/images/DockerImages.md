
# راهنمای کامل تنظیم Nexus Docker Hosted و Push کردن Image‌ها

این فایل شامل دو بخش اصلی است: مراحل تنظیم **Nexus Docker Hosted Repository** و توضیحات مربوط به HTTPS و Push کردن Image‌های موجود.

---

## **1. تنظیم Docker Hosted Repository در Nexus**

### **1.1 پیش‌نیازها**
1. اطمینان حاصل کنید که Nexus نصب و اجرا شده است.
2. یک نسخه Docker نصب‌شده روی سیستم خود داشته باشید.
3. دسترسی به پنل مدیریتی Nexus داشته باشید.

### **1.2 تنظیم Docker Hosted Repository در Nexus**
#### ایجاد مخزن Docker Hosted:
1. وارد رابط کاربری Nexus شوید (پیش‌فرض: `http://<NEXUS_IP>:8081`).
2. به بخش **Repositories** بروید:
   - از منوی کناری، گزینه **Administration > Repository** را انتخاب کنید.
3. روی **Create Repository** کلیک کنید.
4. نوع مخزن را **Docker (hosted)** انتخاب کنید.
5. تنظیمات زیر را اعمال کنید:
   - **Name:** یک نام مناسب برای مخزن (مثلاً `docker-hosted`).
   - **HTTP Port:** یک پورت آزاد انتخاب کنید (مثلاً `5000`).
   - **Blob Store:** یک Blob Store مناسب انتخاب کنید یا یک Blob Store جدید ایجاد کنید.
   - **Enable Docker V1 API:** در صورت نیاز، فعال کنید (در حالت عادی غیرضروری است).
6. روی **Create Repository** کلیک کنید.

---

## **2. پیکربندی Docker برای کار با Nexus**

### **2.1 اضافه کردن Nexus به تنظیمات Docker (برای HTTP)**
اگر مخزن Nexus با پروتکل HTTP تنظیم شده است، باید آن را به‌عنوان یک **insecure registry** تعریف کنید.

1. فایل تنظیمات Docker را باز کنید:
   ```bash
   sudo nano /etc/docker/daemon.json
   ```
2. محتوای زیر را اضافه کنید:
   ```json
   {
     "insecure-registries": ["<NEXUS_IP>:5000"]
   }
   ```
   - **<NEXUS_IP>:5000:** آدرس IP و پورتی که برای Docker Hosted Repository در Nexus تنظیم کرده‌اید.

3. سرویس Docker را ریستارت کنید:
   ```bash
   sudo systemctl restart docker
   ```

---

### **2.2 تنظیم Nexus برای HTTPS**
اگر مخزن Nexus از HTTPS استفاده می‌کند:
1. نیازی به تنظیم `insecure-registries` در فایل `daemon.json` نیست.
2. مطمئن شوید گواهی SSL به درستی تنظیم شده است.
3. اگر از گواهی Self-Signed استفاده می‌کنید، آن را در مسیر `/etc/docker/certs.d/<NEXUS_IP>:5000/ca.crt` قرار دهید و Docker را ریستارت کنید:
   ```bash
   sudo systemctl restart docker
   ```

---

## **3. تست ارتباط با Nexus**
### **3.1 ورود به Nexus از Docker:**
با دستور زیر به Nexus لاگین کنید:
```bash
docker login <NEXUS_IP>:5000
```
- نام کاربری و رمز عبور Nexus را وارد کنید.
- در صورت موفقیت، پیام `Login Succeeded` نمایش داده می‌شود.

### **3.2 ساخت و Push یک Image تستی:**
1. یک Image ساده بسازید:
   ```bash
   docker build -t <NEXUS_IP>:5000/my-app:1.0 .
   ```
   - **<NEXUS_IP>:5000:** آدرس و پورت Nexus.
   - **my-app:1.0:** نام و نسخه Image.

2. Image را به Nexus Push کنید:
   ```bash
   docker push <NEXUS_IP>:5000/my-app:1.0
   ```

3. برای اطمینان از موفقیت، Image را دوباره از Nexus Pull کنید:
   ```bash
   docker pull <NEXUS_IP>:5000/my-app:1.0
   ```

---

## **4. تگ زدن Image‌های موجود و Push کردن آن‌ها**

اگر قبلاً یک Image در Docker دارید و می‌خواهید آن را به Nexus Push کنید:

### **4.1 بررسی Image‌های موجود:**
با دستور زیر لیست Image‌های موجود را مشاهده کنید:
```bash
docker images
```
خروجی نمونه:
```
REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
my-local-image      latest    123456abcde    2 days ago      150MB
```

### **4.2 تگ زدن Image موجود:**
برای تگ زدن Image موجود جهت Push به Nexus:
```bash
docker tag <IMAGE_ID> <NEXUS_IP>:5000/<REPOSITORY_NAME>:<TAG>
```
مثال:
```bash
docker tag 123456abcde <NEXUS_IP>:5000/my-repo/my-local-image:1.0
```
- **<IMAGE_ID>:** شناسه Image موجود.
- **<NEXUS_IP>:5000:** آدرس و پورت Nexus.
- **my-repo/my-local-image:** نام Image در Nexus.
- **1.0:** نسخه (Tag) Image.

### **4.3 Push کردن Image به Nexus:**
```bash
docker push <NEXUS_IP>:5000/my-repo/my-local-image:1.0
```

---

## **5. نکات مهم**
1. **HTTPS یا HTTP:**
   - برای HTTPS، Docker به تنظیمات خاصی نیاز ندارد، مگر اینکه گواهی معتبر نباشد.
   - برای HTTP، باید `insecure-registries` را در `daemon.json` پیکربندی کنید.

2. **تگ زدن Image:**
   - حتماً آدرس و پورت Nexus و نام مخزن را در تگ Image مشخص کنید.

3. **گواهی Self-Signed:**
   - اگر از گواهی Self-Signed استفاده می‌کنید، باید آن را به Docker اضافه کنید.

---

## **6. جمع‌بندی**
- یک Docker Hosted Repository در Nexus ایجاد کنید.
- Nexus را برای HTTPS یا HTTP تنظیم کنید.
- Imageها را از Docker Push و Pull کنید.
- اگر مشکلی پیش آمد، لاگ‌های Docker و Nexus را بررسی کنید.

اگر سوالی دارید یا نیاز به توضیحات بیشتری دارید، اطلاع دهید!
