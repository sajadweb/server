## Tag create

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

یک مثال واقعی از چرخه عمر پروژه:
کامیت‌های اولیه: (تگ نداریم، فقط داریم کد می‌زنیم)
تکمیل صفحه لاگین: git tag -a v0.1.0 -m "Login page completed" 
←
 (شماره 0 یعنی هنوز به نسخه یک نرسیدیم)
اضافه کردن سبد خرید: git tag -a v0.2.0 -m "Shopping cart added"
رفع باگ دکمه پرداخت: git tag -a v0.2.1 -m "Fixed payment button bug"
پروژه آماده انتشار برای کاربرا شد: git tag -a v1.0.0 -m "First official public release"
شش ماه بعد، اضافه کردن حالت تیره (Dark Mode): git tag -a v1.1.0 -m "Added dark mode feature"
نکته اضافه: اگر پروژه‌هات بزرگ بشه، می‌تونی از پکیج‌هایی مثل standard-version یا semantic-release استفاده کنی که خودش با توجه به کامیت‌هات، بهت پیشنهاد می‌ده ورژن بعدی چنده و حتی فایل CHANGELOG.md رو هم خودکار آپدیت می‌کنه. اما برای الان، همان روش دستی با git tag عالی و استاندارده!
