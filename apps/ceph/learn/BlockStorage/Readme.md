# نمودار کامل مسیر داده در Ceph Block Storage (RBD)

## نمودار توالی عملیات (Sequence Diagram)
```mermaid
sequenceDiagram
    participant Client
    participant Kernel
    participant librbd
    participant OSD
    
    Client->>Kernel: ارسال I/O Request (خواندن/نوشتن)
    Kernel->>librbd: تبدیل به درخواست‌های Ceph
    librbd->>OSD: ارسال عملیات به PGهای مربوطه
    OSD->>OSD: تکثیر داده (Replication)
    OSD->>librbd: تأیید انجام عملیات
    librbd->>Kernel: بازگرداندن نتیجه
    Kernel->>Client: پاسخ نهایی
