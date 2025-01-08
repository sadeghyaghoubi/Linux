

# سرویس‌هایی که Logstash می‌تواند با آن‌ها ارتباط برقرار کند


## نکته مهم: 
درصورت ایجاد فایل لاگ استش جدید حتما باید در پوشه pipline فایل مربوطه را ایجاد کرد .

## 1. دریافت داده‌ها (Inputs)  
Logstash می‌تواند داده‌ها را از منابع زیر دریافت کند:

### منابع رایج:
- **فایل‌ها (File)**:
  خواندن داده‌ها از فایل‌های لاگ.
  ```plaintext
  input {
    file {
      path => "/var/log/my-app.log"
    }
  }
  ```

- **TCP/UDP**:
  دریافت داده‌ها از طریق پروتکل‌های شبکه.
  ```plaintext
  input {
    tcp {
      port => 5044
    }
  }
  ```

- **Beats**:
  دریافت داده‌ها از **Filebeat، Metricbeat** و دیگر ابزارهای خانواده Beats.
  ```plaintext
  input {
    beats {
      port => 5044
    }
  }
  ```

- **Kafka**:
  اتصال به Apache Kafka برای مصرف پیام‌ها.
  ```plaintext
  input {
    kafka {
      topics => ["my-topic"]
      bootstrap_servers => "localhost:9092"
    }
  }
  ```

- **HTTP/HTTP Poller**:
  دریافت داده‌ها از طریق درخواست‌های HTTP یا خواندن از یک API.
  ```plaintext
  input {
    http {
      port => 8080
    }
  }
  ```

- **Redis**:
  مصرف داده‌ها از صف Redis.
  ```plaintext
  input {
    redis {
      host => "localhost"
      data_type => "list"
      key => "logstash"
    }
  }
  ```

### سایر منابع:
- RabbitMQ
- Amazon S3
- Google Cloud Pub/Sub
- Azure Event Hub
- JDBC (برای خواندن داده از دیتابیس‌ها)
- Stdin (برای داده‌های ورودی خط فرمان)

---

## 2. ارسال داده‌ها (Outputs)  
Logstash می‌تواند داده‌ها را به سرویس‌ها و مقصدهای زیر ارسال کند:

### مقاصد رایج:
- **Elasticsearch**:
  ارسال داده‌ها برای ذخیره و تحلیل.
  ```plaintext
  output {
    elasticsearch {
      hosts => ["localhost:9200"]
      index => "my-index"
    }
  }
  ```

- **Kafka**:
  ارسال داده‌ها به Apache Kafka.
  ```plaintext
  output {
    kafka {
      topic_id => "my-topic"
      bootstrap_servers => "localhost:9092"
    }
  }
  ```

- **File**:
  ذخیره داده‌های پردازش شده در فایل.
  ```plaintext
  output {
    file {
      path => "/var/log/processed-data.log"
    }
  }
  ```

- **HTTP**:
  ارسال داده به یک API یا endpoint.
  ```plaintext
  output {
    http {
      url => "http://localhost:3000/endpoint"
      http_method => "post"
    }
  }
  ```

- **Redis**:
  ارسال داده‌ها به صف Redis.
  ```plaintext
  output {
    redis {
      host => "localhost"
      data_type => "list"
      key => "logstash-output"
    }
  }
  ```

### سایر مقاصد:
- Amazon S3
- Google Cloud Storage
- RabbitMQ
- Email
- Stdout (برای مشاهده داده در کنسول)

---

## 3. ارتباط با پایگاه‌های داده
Logstash می‌تواند داده‌ها را به پایگاه داده ارسال کند یا از آن‌ها بخواند:
- **MySQL، PostgreSQL، Oracle، SQL Server**:
  از طریق پلاگین **JDBC**.
  ```plaintext
  input {
    jdbc {
      jdbc_connection_string => "jdbc:mysql://localhost:3306/mydb"
      jdbc_user => "user"
      jdbc_password => "password"
      statement => "SELECT * FROM logs"
    }
  }
  ```

- **MongoDB**:
  استفاده از پلاگین‌های جانبی.

---

## 4. ادغام با سیستم‌های ابری
Logstash می‌تواند با سرویس‌های ابری ارتباط داشته باشد:
- **Amazon Web Services (AWS):**
  - S3 (ذخیره و خواندن داده)
  - CloudWatch
  - Kinesis

- **Microsoft Azure:**
  - Event Hub
  - Blob Storage

- **Google Cloud:**
  - Pub/Sub
  - Storage

---

## 5. جمع‌بندی
Logstash به دلیل معماری مبتنی بر پلاگین، می‌تواند تقریباً با هر سیستمی که از انتقال داده‌ها پشتیبانی می‌کند ارتباط برقرار کند. برای پروژه‌های مختلف، کافی است از ترکیب مناسب **input**، **filter** و **output** استفاده کنید. اگر نیاز به راهنمایی در مورد یک سرویس خاص دارید، بگویید!
