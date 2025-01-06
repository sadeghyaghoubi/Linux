
# راهنمای تنظیم مخزن YUM در Nexus

این فایل مراحل تنظیم مخزن YUM در Nexus و آپلود پکیج‌های RPM از طریق یک فایل ISO را توضیح می‌دهد.

---

## **1. ورود به Nexus**
1. وارد Nexus شوید.
2. با نام کاربری و رمز عبور لاگین کنید.

---

## **2. ایجاد Blob Store**
1. به مسیر **Administration > Repository > Blob Stores** بروید.
2. گزینه **Create Blob Store** را انتخاب کنید.
3. نوع Blob Store را **File** تنظیم کنید و یک نام دلخواه انتخاب کنید.
4. تغییرات را ذخیره کنید.

---

## **3. ایجاد مخزن YUM (Hosted)**
1. به مسیر **Repository > Create Repository** بروید.
2. نوع مخزن را **YUM (Hosted)** انتخاب کنید.
3. دو مخزن با تنظیمات زیر ایجاد کنید:
   - **osname-baseos**
   - **osname-appstream**

---

## **4. دانلود فایل ISO**
1. فایل ISO سیستم‌عامل موردنظر (مانند Rocky Linux) را دانلود کنید.

---

## **5. آپلود فایل ISO روی سرور ESXi**
1. وارد محیط سرور ESXi شوید.
2. فایل ISO را در یکی از دیتاستورهای سرور آپلود کنید.

---

## **6. افزودن ISO به ماشین مجازی**
1. وارد تنظیمات ماشین مجازی لینوکس خود شوید.
2. فایل ISO آپلودشده را به عنوان CD/DVD به ماشین اضافه کنید.

---

## **7. مونت کردن فایل ISO**
1. در ماشین لینوکس، دایرکتوری برای مونت کردن ایجاد کنید:
   ```bash
   mkdir /mnt/rocky
   ```
2. فایل ISO را مونت کنید:
   ```bash
   mount /dev/sr0 /mnt/rocky
   ```
3. فایل‌های ISO به طور خودکار در مسیر `/mnt/rocky` در دسترس خواهند بود.

---

## **8. ایجاد دایرکتوری‌های AppStream و BaseOS**
1. دو دایرکتوری جدید برای پکیج‌ها ایجاد کنید:
   ```bash
   mkdir /mnt/rocky/AppStream
   mkdir /mnt/rocky/BaseOS
   ```

---

## **9. حلقه for برای آپلود پکیج‌ها به Nexus**

### **آپلود پکیج‌های AppStream**
```bash
for i in $(find /mnt/rocky/AppStream/Packages -name *.rpm); do
  curl -v -u admin:admin --upload-file "$i" http://<NEXUS_IP>/repository/osname-appstream/
done
```

### **آپلود پکیج‌های BaseOS**
```bash
for i in $(find /mnt/rocky/BaseOS/Packages -name *.rpm); do
  curl -v -u admin:admin --upload-file "$i" http://<NEXUS_IP>/repository/osname-baseos/
done
```

- **<NEXUS_IP>:** آدرس IP سرور Nexus را وارد کنید.
- **admin:admin:** اطلاعات لاگین Nexus (در صورت نیاز تغییر دهید).

---

## **10. پایان**
با انجام مراحل بالا، تمامی پکیج‌ها به مخزن YUM در Nexus آپلود شده و آماده استفاده خواهند بود.

