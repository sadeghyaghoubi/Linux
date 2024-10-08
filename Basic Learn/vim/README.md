
# ویرایشگر متن Vim

**Vim** (مخفف "Vi IMproved") یک ویرایشگر متن قدرتمند و بسیار قابل تنظیم است که بر اساس ویرایشگر قدیمی‌تر `vi` ساخته شده است. Vim به دلیل امکانات گسترده، سرعت بالا، و قابلیت کار در محیط‌های ترمینال، محبوبیت زیادی در میان برنامه‌نویسان و کاربران حرفه‌ای دارد.

## ویژگی‌های کلیدی Vim

1. **ویرایشگر حالت‌مند**: Vim دارای حالت‌های مختلفی است که هر یک برای وظایف خاصی استفاده می‌شود:
   - **Normal mode (حالت عادی)**: این حالت برای حرکت در متن و اجرای دستورات ویرایشی استفاده می‌شود. در این حالت نمی‌توانید مستقیماً متن وارد کنید.
   - **Insert mode (حالت درج)**: در این حالت می‌توانید متن را ویرایش و وارد کنید. این حالت شبیه به حالت عادی ویرایشگرهای دیگر است.
   - **Visual mode (حالت دیداری)**: برای انتخاب متن استفاده می‌شود. انتخاب‌ها می‌توانند به صورت کاراکتری، خطی، یا بلوکی باشند.
   - **Command-line mode (حالت خط فرمان)**: برای اجرای دستورات ویژه، مانند جستجو، جایگزینی، ذخیره‌سازی و خروج از فایل استفاده می‌شود.

2. **سفارشی‌سازی و پلاگین‌ها**: Vim از یک فایل تنظیمات به نام `.vimrc` برای سفارشی‌سازی رفتار و ظاهر خود استفاده می‌کند. همچنین، از پلاگین‌های مختلفی برای گسترش قابلیت‌های خود پشتیبانی می‌کند.

3. **دستورات ویرایشی قدرتمند**: Vim به شما اجازه می‌دهد با استفاده از دستورات کوتاه و ترکیبی، عملیات پیچیده‌ای را روی متن انجام دهید.

## کلیدهای کاربردی در Vim

### حرکت در متن
- **h**: حرکت به چپ
- **j**: حرکت به پایین
- **k**: حرکت به بالا
- **l**: حرکت به راست
- **w**: حرکت به کلمه بعدی
- **b**: حرکت به ابتدای کلمه قبلی
- **0**: رفتن به ابتدای خط
- **$**: رفتن به انتهای خط

### وارد شدن به حالت‌های مختلف
- **i**: ورود به حالت درج از محل فعلی
- **I**: ورود به حالت درج در ابتدای خط
- **a**: ورود به حالت درج بعد از کاراکتر فعلی
- **A**: ورود به حالت درج در انتهای خط
- **v**: ورود به حالت دیداری برای انتخاب کاراکترها
- **V**: ورود به حالت دیداری برای انتخاب خطوط
- **Ctrl+v**: ورود به حالت دیداری بلوکی

### ویرایش و تغییر
- **x**: حذف کاراکتر در محل کرسر
- **dd**: حذف خط جاری
- **yy**: کپی کردن (yank) خط جاری
- **p**: چسباندن (paste) متن کپی شده
- **u**: لغو (undo) آخرین تغییر
- **Ctrl+r**: بازگرداندن (redo) آخرین لغو

### دستورات خط فرمان
- **:w**: ذخیره فایل جاری
- **:q**: خروج از فایل جاری
- **:wq**: ذخیره و خروج
- **:q!**: خروج بدون ذخیره تغییرات
- **:e filename**: باز کردن فایل جدید
- **:set nu**: نمایش شماره خطوط
- **:set nonu**: غیرفعال کردن نمایش شماره خطوط

### جستجو
- **/**: جستجو به سمت جلو
- **?**: جستجو به سمت عقب
- **n**: رفتن به نتیجه بعدی جستجو
- **N**: رفتن به نتیجه قبلی جستجو

## یادگیری و تمرین

یادگیری Vim به دلیل داشتن منحنی یادگیری تند، نیاز به تمرین و استفاده مداوم دارد. برای یادگیری بهتر، می‌توانید از `vimtutor` استفاده کنید که یک آموزش تعاملی است و با نصب Vim در دسترس است. همچنین منابع آنلاین، کتاب‌ها و دوره‌های آموزشی زیادی برای یادگیری Vim وجود دارند.
