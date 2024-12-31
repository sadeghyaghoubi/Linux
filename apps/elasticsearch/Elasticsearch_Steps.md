
# مراحل کار با Elasticsearch

## 1. ایجاد ایندکس (Index)

ایندکس‌ها در Elasticsearch معادل جدول‌ها در دیتابیس رابطه‌ای هستند. ابتدا باید یک ایندکس برای ذخیره داده‌ها ایجاد کنید.

### دستور ایجاد ایندکس:
```json
PUT /my-index-000001
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  }
}
```
- `number_of_shards`: تعداد شاردها برای توزیع داده‌ها.
- `number_of_replicas`: تعداد نسخه‌های کپی برای تحمل خطا.

---

## 2. ایجاد Rollover

Rollover برای مدیریت خودکار ایندکس‌ها استفاده می‌شود. این قابلیت زمانی مفید است که داده‌ها حجم زیادی دارند و نیاز به تقسیم‌بندی (rotation) دارند.

### مراحل:
1. **ایجاد ایندکس اولیه با نام مستعار (Alias):**
   ```json
   PUT /my-index-000001
   {
     "aliases": {
       "my-index": {
         "is_write_index": true
       }
     }
   }
   ```

2. **تعریف rollover:**
   ```json
   POST /my-index/_rollover
   {
     "conditions": {
       "max_age": "30d",
       "max_docs": 1000000
     }
   }
   ```
   - `max_age`: حداکثر سن ایندکس.
   - `max_docs`: حداکثر تعداد مستندات.

---

## 3. ایجاد Index Pattern

Index Pattern برای جستجو و تحلیل در Kibana استفاده می‌شود و به شما امکان می‌دهد داده‌های مشابه را مشاهده کنید.

### مراحل:
1. وارد Kibana شوید.
2. به مسیر **Management > Stack Management > Index Patterns** بروید.
3. یک Index Pattern مانند `my-index-*` ایجاد کنید.
4. فیلد زمان (timestamp) را تنظیم کنید.

---

## 4. ایجاد Template

Templates برای تنظیمات پیش‌فرض ایندکس‌ها استفاده می‌شوند تا نیازی به تنظیم دستی نباشد.

### ایجاد Template:
```json
PUT /_index_template/my-template
{
  "index_patterns": ["my-index-*"],
  "template": {
    "settings": {
      "number_of_shards": 1
    },
    "mappings": {
      "properties": {
        "timestamp": {
          "type": "date"
        },
        "message": {
          "type": "text"
        }
      }
    }
  },
  "priority": 1
}
```
- `index_patterns`: ایندکس‌هایی که این template اعمال شود.
- `settings`: تنظیمات ایندکس.
- `mappings`: نوع فیلدها و ساختار داده.

---

## 5. ایجاد ILM (Index Lifecycle Management)

ILM برای مدیریت چرخه عمر داده‌هاست، شامل مراحل hot, warm, cold، و delete.

### مراحل:
1. **ایجاد Policy:**
   ```json
   PUT /_ilm/policy/my-policy
   {
     "policy": {
       "phases": {
         "hot": {
           "actions": {
             "rollover": {
               "max_size": "50gb",
               "max_age": "7d"
             }
           }
         },
         "delete": {
           "min_age": "30d",
           "actions": {
             "delete": {}
           }
         }
       }
     }
   }
   ```
   - `hot`: برای نوشتن و خواندن سریع.
   - `delete`: حذف ایندکس پس از 30 روز.

2. **اتصال Policy به Template:**
   ```json
   PUT /_index_template/my-template
   {
     "index_patterns": ["my-index-*"],
     "template": {
       "settings": {
         "index.lifecycle.name": "my-policy",
         "index.lifecycle.rollover_alias": "my-index"
       }
     }
   }
   ```

---

## 6. ایجاد و ارسال داده به ایندکس

برای ارسال داده، از API زیر استفاده کنید:
```json
POST /my-index/_doc
{
  "timestamp": "2024-12-31T12:00:00",
  "message": "Hello, Elasticsearch!"
}
```

---

## 7. جستجو در ایندکس

برای جستجوی داده‌ها:
```json
GET /my-index-*/_search
{
  "query": {
    "match": {
      "message": "Hello"
    }
  }
}
```

---

## ترتیب اجرای مراحل:
1. **ایجاد Template**
2. **ایجاد ILM Policy**
3. **ایجاد ایندکس اولیه و Alias**
4. **تعریف Rollover**
5. **ایجاد Index Pattern در Kibana**
6. **ارسال و جستجوی داده‌ها**
