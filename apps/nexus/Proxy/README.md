
# تنظیم Proxy Repository در Nexus برای پکیج‌های خارجی

در این فایل مراحل تنظیم یک مخزن Proxy در Nexus و استفاده از آن برای نصب پکیج‌هایی مانند `vim` توضیح داده شده است.

---

## **قابلیت Proxy در Nexus**
Proxy Repository در Nexus به شما امکان می‌دهد تا درخواست‌های کاربران به مخازن خارجی را مدیریت کنید. با تعریف یک Proxy Repository:
- Nexus به عنوان واسطه بین کاربران و مخازن خارجی عمل می‌کند.
- پکیج‌ها کش می‌شوند و در درخواست‌های بعدی مستقیماً از Nexus ارائه می‌شوند.

---

## **مراحل تنظیم یک Proxy Repository**

### **1. ایجاد Proxy Repository در Nexus**
1. وارد رابط کاربری Nexus شوید.
2. به مسیر **Repository > Create Repository** بروید.
3. نوع مخزن مناسب را انتخاب کنید:
   - **APT Proxy:** برای مخازن Debian/Ubuntu.
   - **YUM Proxy:** برای مخازن RedHat/CentOS/RockyLinux.
   - **Docker Proxy:** برای مخازن Docker Hub.
4. تنظیمات زیر را وارد کنید:
   - **Name:** `proxy-vim` یا هر نام مناسب دیگر.
   - **Remote Storage URL:** آدرس مخزن خارجی.
     - برای پکیج `vim` در مخازن Ubuntu:
       ```
       http://archive.ubuntu.com/ubuntu/
       ```
   - **Blob Store:** یک Blob Store مناسب انتخاب کنید یا جدید بسازید.
   - **Content Caching:** فعال کنید تا فایل‌ها در کش ذخیره شوند.
5. تغییرات را ذخیره کنید.

---

### **2. اضافه کردن Proxy Repository به سیستم مشتری**

#### **APT (Debian/Ubuntu):**
1. فایل APT Source جدیدی ایجاد کنید:
   ```bash
   sudo nano /etc/apt/sources.list.d/nexus-proxy.list
   ```
2. خط زیر را اضافه کنید:
   ```
   deb [trusted=yes] http://<NEXUS_IP>:8081/repository/proxy-vim/ focal main
   ```
   - **<NEXUS_IP>:8081:** آدرس و پورت Nexus.
   - **proxy-vim:** نام مخزن Proxy.

3. مخازن را به‌روزرسانی کنید:
   ```bash
   sudo apt update
   ```

#### **YUM (CentOS/Rocky):**
1. یک فایل جدید در مسیر `/etc/yum.repos.d/` ایجاد کنید:
   ```bash
   sudo nano /etc/yum.repos.d/nexus-proxy.repo
   ```
2. محتوای زیر را وارد کنید:
   ```
   [proxy-vim]
   name=Proxy Repository for Vim
   baseurl=http://<NEXUS_IP>:8081/repository/proxy-vim/
   enabled=1
   gpgcheck=0
   ```
3. کش را پاک کنید و YUM را به‌روزرسانی کنید:
   ```bash
   sudo yum clean all
   sudo yum makecache
   ```

---

### **3. نصب پکیج از Proxy Repository**

#### **APT:**
برای نصب `vim`:
```bash
sudo apt install vim
```

#### **YUM:**
برای نصب `vim`:
```bash
sudo yum install vim
```

---

## **نحوه عملکرد Proxy در زمان نصب**
1. **درخواست اول:**
   - Nexus بررسی می‌کند که آیا پکیج در کش موجود است.
   - اگر موجود نباشد، Nexus درخواست را به مخزن خارجی ارسال کرده و فایل را دانلود و کش می‌کند.
2. **درخواست‌های بعدی:**
   - فایل مستقیماً از کش Nexus ارائه می‌شود.

---

## **مزایای استفاده از Proxy Repository**
1. **کاهش پهنای باند:** فایل‌ها فقط یک بار از مخزن خارجی دانلود می‌شوند.
2. **افزایش سرعت:** کاربران داخلی با سرعت بیشتری به فایل‌ها دسترسی پیدا می‌کنند.
3. **مدیریت بهتر:** کنترل مرکزی بر نسخه‌های پکیج‌ها.
4. **افزایش امنیت:** امکان بررسی و مسدود کردن پکیج‌های خاص.

---

## **جمع‌بندی**
- Proxy Repository ابزاری قدرتمند برای مدیریت دسترسی به مخازن خارجی است.
- می‌توانید با تنظیم Proxy Repository برای پکیج‌هایی مانند `vim`، دسترسی سریع‌تر و بهینه‌تری داشته باشید.
