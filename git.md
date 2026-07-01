## Tag create

```bash
  git tag v0.1.2
```
## Tag create with commit
```bash
  git tag -a v1.0.0 -m "First stable release: Login and Dashboard features added"
```

## List tag

```bash
  git tag
```
## Push one tag
```bash
  git push origin v1.0.0
```
## Push  all
```bash
  git push origin --tags
```

### مرحله چهارم: ساختن Release در گیت‌هاب (رسمی‌ترین روش)
وقتی تگ رو پوش کردی، در واقع ورژن‌بندی فنی انجام شده. اما برای اینکه گیت‌هاب اون رو به عنوان یک "نسخه آماده انتشار" بشناسه (و کاربرا بتونن فایل زیپش رو دانلود کنن یا تغییراتش رو بخونن):

1. به ریپازیتوری‌ت در گیت‌هاب برو.
2. از منوی سمت راست، روی **Releases** کلیک کن.
3. دکمه **Draft a new release** رو بزن.
4. در قسمت **Choose a tag**، روی دکمه کنارش کلیک کن و تگ `v1.0.0` که刚才 فرستادی رو انتخاب کن (یا اونجا هم می‌تونی تگ جدید بسازی).
5. در قسمت **Release title** بنویس: `v1.0.0`
6. در کادر متنی زیرش (Describe this release)، بنویس که در این ورژن چه چیزهایی اضافه یا تغییر کرده (اینطوری شبیه یه لاگ تغییرات یا Changelog میشه).
7. دکمه **Publish release** رو بزن.

---

### یک مثال واقعی از چرخه عمر پروژه:
*   **کامیت‌های اولیه:** (تگ نداریم، فقط داریم کد می‌زنیم)
*   **تکمیل صفحه لاگین:** `git tag -a v0.1.0 -m "Login page completed"` $\leftarrow$ (شماره 0 یعنی هنوز به نسخه یک نرسیدیم)
*   **اضافه کردن سبد خرید:** `git tag -a v0.2.0 -m "Shopping cart added"`
*   **رفع باگ دکمه پرداخت:** `git tag -a v0.2.1 -m "Fixed payment button bug"`
*   **پروژه آماده انتشار برای کاربرا شد:** `git tag -a v1.0.0 -m "First official public release"`
*   **شش ماه بعد، اضافه کردن حالت تیره (Dark Mode):** `git tag -a v1.1.0 -m "Added dark mode feature"`

**نکته اضافه:** اگر پروژه‌هات بزرگ بشه، می‌تونی از پکیج‌هایی مثل `standard-version` یا `semantic-release` استفاده کنی که خودش با توجه به کامیت‌هات، بهت پیشنهاد می‌ده ورژن بعدی چنده و حتی فایل `CHANGELOG.md` رو هم خودکار آپدیت می‌کنه. اما برای الان، همان روش دستی با `git tag` عالی و استاندارده!
