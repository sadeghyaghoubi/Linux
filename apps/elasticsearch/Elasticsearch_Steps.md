
# مراحل کار با Elasticsearch

## 1. ایجاد ایندکس (Index)

ایندکس‌ها در Elasticsearch معادل جدول‌ها در دیتابیس رابطه‌ای هستند. ابتدا باید یک ایندکس برای ذخیره داده‌ها ایجاد کنید.

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

## 2. ایجاد lifecycle

hot :

Rollover : enable

maximum age: 30days

maximum index size : 50 days

index priority: enable

cold:

move data into phase when: 1

force merge : enable 

number of segments:1

index priority : enable 

index priority: 50


cold : disable


delete phase 

move data into phase when: : 30






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

