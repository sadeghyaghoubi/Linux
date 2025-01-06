
# Push کردن Docker Image به Nexus

در این راهنما مراحل Push کردن یک Docker Image به مخزن Hosted در Nexus توضیح داده شده است.

---

## **ایجاد Docker Hosted Repository در Nexus**
1. وارد Nexus شوید.
2. به مسیر **Repository > Create Repository** بروید.
3. **Docker (hosted)** را انتخاب کنید.
4. تنظیمات زیر را اعمال کنید:
   - **Name:** `docker-hosted` یا هر نام دلخواه.
   - **HTTP Port:** یک پورت آزاد، مانند `5000`.
5. تغییرات را ذخیره کنید.

---

## **ساخت و تگ زدن Image**
1. **ساخت یک Image جدید:**
   اگر یک Dockerfile دارید، Image را با دستور زیر بسازید:
   ```bash
   docker build -t my-image:1.0 .
   ```

2. **تگ زدن Image برای Nexus:**
   Image باید با نام Repository شما تگ شود:
   ```bash
   docker tag my-image:1.0 <NEXUS_IP>:5000/my-image:1.0
   ```
   - **<NEXUS_IP>:5000:** آدرس و پورت مخزن شما.
   - **my-image:1.0:** نام و نسخه Image.

---

## **Push کردن Image به Nexus**
1. به Nexus لاگین کنید:
   ```bash
   docker login <NEXUS_IP>:5000
   ```
   - اطلاعات کاربری Nexus خود را وارد کنید.
   
2. Image را به Nexus Push کنید:
   ```bash
   docker push <NEXUS_IP>:5000/my-image:1.0
   ```

---

## **بررسی Image در Nexus**
1. وارد رابط کاربری Nexus شوید.
2. به مخزن Docker Hosted (`docker-hosted`) بروید.
3. Image آپلودشده را مشاهده کنید.

---

## **Pull کردن Image از Nexus**
برای اطمینان از موفقیت Push:
1. Image را از Nexus Pull کنید:
   ```bash
   docker pull <NEXUS_IP>:5000/my-image:1.0
   ```

2. یک کانتینر از این Image ایجاد کنید:
   ```bash
   docker run -d --name test-container <NEXUS_IP>:5000/my-image:1.0
   ```

---

## **نکات مهم**
- اطمینان حاصل کنید که Nexus به درستی تنظیم شده و از Docker Hosted Repository استفاده می‌کنید.
- برای امنیت بیشتر، استفاده از HTTPS توصیه می‌شود.
- اگر خطایی در Push یا Pull رخ داد، لاگ‌های Nexus و Docker را بررسی کنید.

