# با روشن شدن ماشین اول میره دیتا مربوط به این فایل رو میخونه و اجرا میکنه

نمونه‌ای از یک فایل .bash_profile

# تنظیم PATH
```bash
export PATH=$PATH:$HOME/bin

# تنظیم تاریخچه دستورات
export HISTSIZE=1000
export HISTFILESIZE=2000

# نمایش یک پیام خوش‌آمدگویی
echo "Welcome to your shell session, $USER!"

# منابع‌دهی به فایل .bashrc برای تنظیمات بیشتر
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
```
