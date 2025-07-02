برای بستن تمام پورت‌ها به جز پورت 80 در اوبونتو با استفاده از `ufw` (Uncomplicated Firewall)، می‌توانید از دستورات زیر استفاده کنید:

### 1. ابتدا `ufw` را فعال کنید (اگر قبلاً فعال نشده است):
```bash
sudo ufw enable
```

### 2. تمام ترافیک ورودی را به طور پیش‌فرض مسدود کنید:
```bash
sudo ufw default deny incoming
```

### 3. پورت 80 (HTTP) را برای ترافیک ورودی مجاز کنید:
```bash
sudo ufw allow 80/tcp
```

### 4. وضعیت `ufw` را بررسی کنید تا مطمئن شوید قوانین به درستی اعمال شده‌اند:
```bash
sudo ufw status verbose
```

### خروجی مورد انتظار:
```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
80/tcp                     ALLOW       Anywhere
80/tcp (v6)                ALLOW       Anywhere (v6)
```

### توضیحات:
- `default deny incoming`: تمام ترافیک ورودی را به طور پیش‌فرض مسدود می‌کند.
- `allow 80/tcp`: فقط پورت 80 (مورد استفاده برای HTTP) را باز می‌گذارد.
- اگر نیاز به پورت دیگری دارید (مثلاً 443 برای HTTPS)، باید آن را نیز مجاز کنید:
  ```bash
  sudo ufw allow 443/tcp
  ```


