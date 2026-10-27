# راهنمای رفع خطای 429 در 9Router

این راهنما مراحلی را مستند می‌کند که روی یک سیستم ویندوز آزمایش شده‌اند. در این تست، مدل‌های `mimo-v2.6-flash-free` و `deepseek-v4.1-flash-free` پاسخ HTTP `200` دادند.

> **توجه:** این روش تضمین نمی‌کند که خطای 429 برای همه برطرف شود. اگر سهمیه واقعاً تمام شده باشد یا سرویس محدودیت IP اعمال کرده باشد، ممکن است خطا ادامه پیدا کند.

## پیش‌نیازها

روی ویندوز، موارد زیر را نصب داشته باشید:

- Git
- Node.js و npm
- Bun

برای نصب Bun در PowerShell اجرا کنید:

```powershell
powershell -c "irm bun.sh/install.ps1|iex"
```

پس از نصب Bun، یک پنجره جدید PowerShell باز کنید.

## ۱. دریافت نسخه آزمایش‌شده

دستورات زیر را در PowerShell اجرا کنید:

```powershell
git clone https://github.com/zeusire/9router.git 9router-429-fix
cd 9router-429-fix
npm install
npm install --no-save wreq-js playwright-core
```

کامیت [`2048fc6e`](https://github.com/zeusire/9router/commit/2048fc6e) همان نسخه‌ای است که در تست اصلی این راهنما استفاده شد.

## ۲. تنظیم متغیرهای محیطی

در همان پنجره PowerShell و داخل پوشه پروژه، دستورات زیر را اجرا کنید:

```powershell
$env:DATA_DIR = "$env:APPDATA\9router"
$env:BASE_URL = "http://localhost:20128"
$env:NEXT_PUBLIC_BASE_URL = "http://localhost:20128"
$env:OPENCODE_TRANSPORT = "auto"
```

## ۳. ساخت و اجرای نسخه Production

برای ساخت نسخه Production اجرا کنید:

```powershell
npm run build
```

منتظر بمانید تا فرایند Build با موفقیت تمام شود. سپس سرور را اجرا کنید:

```powershell
npm run start -- --hostname 127.0.0.1 --port 20128
```

داشبورد را در مرورگر باز کنید:

**http://localhost:20128**

تا زمانی که از 9Router استفاده می‌کنید، پنجره PowerShell را باز نگه دارید. برای متوقف کردن سرور، `Ctrl+C` را فشار دهید.

## ۴. استفاده‌های بعدی

پس از یک Build موفق، برای استفاده روزمره نیازی به نصب دوباره وابستگی‌ها یا اجرای مجدد Build نیست؛ مگر اینکه کد یا وابستگی‌های پروژه تغییر کرده باشند.

هر بار که خواستید از 9Router استفاده کنید، PowerShell را باز کرده و دستورات زیر را اجرا کنید:

```powershell
cd "$HOME\9router-429-fix"

$env:DATA_DIR = "$env:APPDATA\9router"
$env:BASE_URL = "http://localhost:20128"
$env:NEXT_PUBLIC_BASE_URL = "http://localhost:20128"
$env:OPENCODE_TRANSPORT = "auto"

npm run start -- --hostname 127.0.0.1 --port 20128
```

سپس به آدرس زیر بروید:

**http://localhost:20128**

## عیب‌یابی کوتاه

**Build شکست می‌خورد:** متن کامل خطا را بررسی کنید و مطمئن شوید نصب وابستگی‌ها بدون خطا انجام شده است.

**هنوز خطای 429 دریافت می‌کنید:** این روش سهمیه تمام‌شده یا محدودیت‌های اعمال‌شده از سمت سرویس یا IP را برطرف نمی‌کند؛ نتیجه برای همه تضمین‌شده نیست.
