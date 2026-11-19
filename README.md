# تارمرد 🕸️ — بازی موبایلی (Capacitor + PWA)

بازی نوسان بین برج‌ها. کل بازی یک فایل است: `www/index.html`.
این پروژه همان فایل را به اپ اندروید/iOS (با Capacitor) و نسخه‌ی وب نصب‌شدنی (PWA) تبدیل می‌کند.

## ساختار

```
www/                    ← خود بازی (index.html) + PWA (manifest, sw.js, icons)
assets/                 ← آیکون و اسپلش برای ساخت خودکار آیکون اپ
capacitor.config.json   ← تنظیمات اپ (appId: com.tarmard.game)
.github/workflows/
  android.yml           ← ساخت خودکار APK با GitHub Actions
  pages.yml             ← انتشار نسخه‌ی وب روی GitHub Pages
```

## ۱) ساخت APK بدون نصب هیچ چیز (ساده‌ترین راه)

1. یک ریپوی جدید در GitHub بساز و همه‌ی فایل‌های این پوشه را در شاخه‌ی `main` آپلود/پوش کن.
2. به تب **Actions** برو. ورک‌فلوی **Build Android APK** خودش اجرا می‌شود (یا Run workflow بزن).
3. بعد از چند دقیقه، در پایین صفحه‌ی همان اجرا، فایل **tarmard-debug-apk** را دانلود کن و `app-debug.apk` را روی گوشی نصب کن.

> این APK از نوع debug است و برای نصب مستقیم روی گوشی مناسب است. برای انتشار در گوگل‌پلی باید نسخه‌ی release با امضای خودت بسازی (بخش ۴).

## ۲) نسخه‌ی وب (PWA) روی GitHub Pages

1. در ریپو: **Settings → Pages → Source: GitHub Actions**.
2. بعد از اجرای ورک‌فلوی **Deploy web version**، بازی روی آدرس `https://USERNAME.github.io/REPO/` بالا می‌آید.
3. روی گوشی در مرورگر باز کن و **Add to Home Screen** بزن؛ مثل یک اپ تمام‌صفحه اجرا می‌شود.

## ۳) ساخت روی کامپیوتر خودت

نیازها: Node 18+، و برای اندروید Android Studio (JDK 17).

```bash
npm install
npx cap add android          # فقط بار اول
npx capacitor-assets generate --android
npx cap sync android
npx cap open android         # در Android Studio: Run ▶
```

iOS (فقط روی مک و با Xcode):

```bash
npx cap add ios
npx capacitor-assets generate --ios
npx cap sync ios
npx cap open ios
```

بعد از هر تغییر در `www/index.html` فقط `npx cap sync` را اجرا کن.

برای تست سریع در مرورگر: `npm run serve`

## ۴) انتشار در گوگل‌پلی

1. در Android Studio: **Build → Generate Signed Bundle / APK** و یک Keystore بساز (آن را جایی امن نگه دار).
2. فایل `.aab` را در Google Play Console آپلود کن.
3. `appId` را در `capacitor.config.json` به شناسه‌ی دلخواه خودت (مثلاً `com.yourname.tarmard`) تغییر بده، قبل از `cap add android`.

## نکته‌ها

- **جهت صفحه:** اپ روی حالت افقی (landscape) قفل می‌شود. برای تغییر، مقدار `sensorLandscape` را در `.github/workflows/android.yml` عوض کن (مثلاً `sensorPortrait` یا حذفش).
- **فونت فارسی:** فونت «وزیرمتن» از Google Fonts لود می‌شود و بدون اینترنت جایش را فونت سیستم می‌گیرد. برای کاملاً آفلاین شدن، فایل‌های `woff2` را در `www/fonts/` بگذار و با `@font-face` در `index.html` معرفی کن.
- **ذخیره‌ی اطلاعات:** رکورد، اسکین و مرحله در `localStorage` ذخیره می‌شود.
- **کنترل‌ها:** لمس/Space = تار و پرش، F = ضربه، E = شتاب، Q = انفجار، T = زمان، Z = سپر، M = قطع صدا.
