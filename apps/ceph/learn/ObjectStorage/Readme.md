# نمودار توالی ذخیره‌سازی Object در Ceph RGW

```mermaid
sequenceDiagram
    participant Client
    participant RGW
    participant OSD
    
    Client->>RGW: PUT /mybucket/myobject
    Note right of RGW: مرحله ۱: دریافت درخواست
    RGW->>RGW: احراز هویت کاربر
    Note right of RGW: مرحله ۲: بررسی مجوزها
    RGW->>OSD: ارسال داده به PGهای مناسب
    Note right of OSD: مرحله ۳: توزیع داده
    OSD->>OSD: تکثیر داده (Replication)
    Note right of OSD: مرحله ۴: ایجاد replicaها
    OSD->>RGW: تأیید ذخیره‌سازی
    Note left of RGW: مرحله ۵: دریافت تأییدیه
    RGW->>Client: HTTP 200 OK
    Note left of Client: مرحله ۶: اتمام عملیات
