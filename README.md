# راهنمای رفع خطای 429 در 9Router 

این راهنما مراحلی را مستند می‌کند که روی یک سیستم ویندوز آزمایش شده‌اند.
در این تست، مدل‌های mimo-v2.6-flash-free و deepseek-v4.1-flash-free
پاسخ HTTP 200 دادند.

> **توجه:** این روش تضمین نمی‌کند خطای 429 برای همه برطرف شود. اگر سهمیه
> واقعاً تمام شده باشد یا سرویس محدودیت IP اعمال کرده باشد، ممکن است خطا
> ادامه پیدا کند.

## پیش‌نیازها

روی ویندوز این موارد را نصب داشته باشید:

-   Git و
Node.js و npm
-   Bun

برای نصب Bun در PowerShell اجرا کنید:
powershell
powershell -c "irm bun.sh/install.ps1|iex"
پس از نصب Bun، یک پنجره جدید PowerShell باز کنید.

## 1. دریافت نسخه آزمایش‌شده

دستورات زیر را در PowerShell اجرا کنید:
powershell
git clone https://github.com/zeusire/9router.git 9router-429-fix
cd 9router-429-fix
git checkout 2048fc6e
npm install
npm install --no-save wreq-js playwright-core
کامیت 2048fc6e همان نسخه‌ای است که در تست اصلی این راهنما استفاده شد.

## 2. تنظیم متغیرهای محیطی

در همان پنجره PowerShell و داخل پوشه پروژه اجرا کنید:
powershell
$env:DATA_DIR = "$env:APPDATA\9router"
$env:BASE_URL = "http://localhost:20128"
$env:NEXT_PUBLIC_BASE_URL = "http://localhost:20128"
$env:OPENCODE_TRANSPORT = "auto"
## 3. ساخت و اجرای Production

برای ساخت نسخه Production اجرا کنید:
powershell
npm run build
منتظر بمانید تا فرایند Build بدون خطا تمام شود. سپس سرور را اجرا کنید:
powershell
npm run start -- --hostname 127.0.0.1 --port 20128
داشبورد را در مرورگر باز کنید:

**http://localhost:20128**

## 4. استفاده‌های بعدی

بعد از اینکه Build با موفقیت انجام شد، برای استفاده‌های روزمره نیازی به
نصب دوباره یا اجرای مجدد Build نیست.

هر بار که خواستید 9Router را اجرا کنید، PowerShell را باز کرده و این
دستورات را وارد کنید:
powershell
cd "$HOME\9router-429-fix"

$env:DATA_DIR = "$env:APPDATA\9router"
$env:BASE_URL = "http://localhost:20128"
$env:NEXT_PUBLIC_BASE_URL = "http://localhost:20128"
$env:OPENCODE_TRANSPORT = "auto"

npm run start -- --hostname 127.0.0.1 --port 20128
سپس به این آدرس بروید:

**http://localhost:20128**

## عیب‌یابی کوتاه

-   **Build شکست می‌خورد:** متن کامل خطا را بررسی کنید و مطمئن شوید نصب
    وابستگی‌ها بدون خطا انجام شده است.
-   **هنوز خطای 429 می‌گیرید:** این روش سهمیه تمام‌شده یا محدودیت‌های سمت
    سرویس/IP را دور نمی‌زند و نتیجه برای همه تضمین‌شده نیست.
