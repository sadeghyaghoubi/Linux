در اسکریپت‌نویسی شل (مانند Bash)، عملگرها برای انجام عملیات مختلف مانند محاسبات ریاضی، مقایسه‌ها، و عملیات منطقی استفاده می‌شوند. این عملگرها به دسته‌های مختلفی تقسیم می‌شوند:

## 1. عملگرهای ریاضی
این عملگرها برای انجام محاسبات عددی استفاده می‌شوند.

### جمع (+): جمع دو عدد.


```
result=$((a + b))
```

### تفریق (-): تفریق دو عدد.
```
result=$((a - b))
```

### ضرب (*): ضرب دو عدد. توجه کنید که برای استفاده در expr باید با \ اسکیپ شود.
```
result=$((a * b))
```

### تقسیم (/): تقسیم دو عدد.

```
result=$((a / b))
```


### باقی‌مانده (٪): محاسبه باقی‌مانده تقسیم.

```
result=$((a % b))
```


## 2. عملگرهای مقایسه
این عملگرها برای مقایسه مقادیر عددی یا رشته‌ای استفاده می‌شوند.

### مقایسه عددی
### برابر بودن (-eq)

```
if [ "$a" -eq "$b" ]; then
    echo "a برابر b است"
fi
```

### مساوی نبودن (-ne)

```
if [ "$a" -ne "$b" ]; then
    echo "a برابر b نیست"
fi

```


### بزرگتر بودن (-gt)

```
if [ "$a" -gt "$b" ]; then
    echo "a بزرگتر از b است"
fi

```


### بزرگتر یا مساوی بودن (-ge)

```
if [ "$a" -ge "$b" ]; then
    echo "a بزرگتر یا مساوی b است"
fi
```

### کوچکتر بودن (-lt)

```
if [ "$a" -lt "$b" ]; then
    echo "a کوچکتر از b است"
fi
```

### کوچکتر یا مساوی بودن (-le)

```
if [ "$a" -le "$b" ]; then
    echo "a کوچکتر یا مساوی b است"
fi
```

### مقایسه رشته‌ای
### برابر بودن (= یا == در [[ ]])

```
if [ "$a" = "$b" ]; then
    echo "رشته‌ها برابرند"
fi
```


### مساوی نبودن (!=)

```
if [ "$a" != "$b" ]; then
    echo "رشته‌ها برابر نیستند"
fi
```

## 3. عملگرهای منطقی
برای ترکیب یا معکوس کردن شرایط استفاده می‌شوند.

### و (-a یا && در [[ ]]): شرط صحیح اگر هر دو بخش صحیح باشند.

```
if [ "$a" -eq 1 -a "$b" -eq 2 ]; then
    echo "هر دو شرط صحیح هستند"
fi
```


### یا (-o یا || در [[ ]]): شرط صحیح اگر یکی از بخش‌ها صحیح باشد.

```
if [ "$a" -eq 1 -o "$b" -eq 2 ]; then
    echo "حداقل یکی از شرایط صحیح است"
fi
```


### نفی (!): معکوس کردن یک شرط.

```
if [ ! -d "/path/to/dir" ]; then
    echo "پوشه وجود ندارد"
fi
```




## 4. عملگرهای فایل
برای بررسی ویژگی‌های فایل‌ها استفاده می‌شوند.

### وجود داشتن فایل (-e)

```
if [ -e "/path/to/file" ]; then
    echo "فایل وجود دارد"
fi
```


### فایل عادی (-f)

```
if [ -f "/path/to/file" ]; then
    echo "فایل عادی است"
fi
```

## پوشه (-d)

```
if [ -d "/path/to/dir" ]; then
    echo "این یک پوشه است"
fi
```


## اجازه نوشتن (-w)

```
if [ -w "/path/to/file" ]; then
    echo "فایل قابل نوشتن است"
fi

```

این عملگرها ابزارهای اساسی در اسکریپت‌نویسی شل هستند که به شما امکان می‌دهند شرایط پیچیده‌ای را بررسی و مدیریت کنید.





