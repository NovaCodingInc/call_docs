### الزامات دسترسی کلاینت
هر درخواست باید شامل هدرهای زیر برای احراز هویت باشد:
- **X-Api-Key**: کلید API که به کلاینت داده شده است.
- **X-Api-Secret**: رمز API برای تأیید هویت کلاینت.

---

**POST /calls/ringing**
- ایجاد لاگ جدید تماس (رویداد زنگ خوردن)
- درخواست:
{
    "customer_phone": "09123456789",
    "agent_phone": "09876543210",
}
- پاسخ: 
201 Created
{
    "id": "RECORD_ID",
    "customer_phone": "09123456789",
    "agent_phone": "09876543210",
    "event": "calling",
    "metadata": {}
}
- خطاها:
400 Bad Request
401 Unauthorized

**PATCH /calls/:id/answered**
- به‌روزرسانی لاگ تماس به حالت پاسخ داده شده. در پارمترهای درخواست باید آیدی دریافت شده از درخواست `ringing` ارسال شود. 
- درخواست:
{
    "call_duration": 30
}
- پاسخ:
200 OK
{
    "id": "RECORD_ID",
    "event": "in-call"
    "metadata": {
    "ADDITIONAL INFO FOR THE CALL"
  }
}
- خطاها:
400 Bad Request
404 Not Found
401 Unauthorized

---

**PATCH /calls/:id/missed**
- به‌روزرسانی لاگ تماس به حالت از دست رفته. در پارمترهای درخواست باید آیدی دریافت شده از درخواست `ringing` ارسال شود.
- درخواست:
{
    "miss_reason": "پاسخ داده نشد" // ADDITIONAL INFORMATION
}
- پاسخ:
200 OK
{
    "id": "RECORD_ID",
    "event": "missed",
    "metadata": {}
}
- خطاها:
400 Bad Request
404 Not Found
401 Unauthorized

---

**PATCH /calls/:id/hungup**
- به‌روزرسانی لاگ تماس به حالت قطع شدن. در پارمترهای درخواست باید آیدی دریافت شده از درخواست `ringing` ارسال شود.
- درخواست:
{
    "call_duration": 120,
    "call_quality": "good"
}
اطلاعات درخواست به عنوان metadata به لاگ اضافه خواهد شد.

- پاسخ:
200 OK
{
    "id": "RECORD_ID",
    "event": "hung-up",
    "metadata": {}
}
- خطاها:
400 Bad Request
404 Not Found
401 Unauthorized

---

**خطاهای عمومی:**
- 400 Bad Request: ساختار درخواست اشتباه است.
- 401 Unauthorized: دسترسی غیرمجاز.
- 403 Forbidden: دسترسی به دلیل IP نامعتبر یا عدم مجوز به منابع مورد نظر امکان‌پذیر نیست.
- 404 Not Found: لاگ تماس پیدا نشد.
