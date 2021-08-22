<div dir="rtl">

# فصل دهم

# استخراج زیرمسئله‌های [1] غیر مرتبط

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/10-Extracting-Unrelated-Subproblems/img-10-1.png">
</p>

مهندسی یعنی شکستن مسئله‌های بزرگ به مسائل کوچک‌تر و قرار
دادن راه حل‌های این مسائل، کنار هم. اعمال این اصل در کد، آن را منسجم‌تر
و برای خواندن ساده‌تر می‌کند.

توصیه این فصل، تلاش برای شناسایی و استخراج زیرمسئله‌های
غیرمرتبط است. منظور ما این است که:

۱. به تابع یا بلوک کد نگاه کرده و از خود بپرسید هدف سطح

    بالای[2] این کد چیست؟

۲. برای هر خط از کد، سوال کنید که آیا این خط مستقیما

    برای رسیدن به آن هدف کار می‌کند؟ و یا اینکه برای حل یک زیرمسئله
    نامرتبط نیاز است که آن را در کد داشته باشیم؟

۳.  اگر خطوط کافی، یک زیرمسئله غیرمرتبط را حل می‌کنند،

    آن کد را استخراج و به چند تابع جداگانه تبدیل کنید.

هرچند استخراج و تبدیل کد به چندین تابع جداگانه، احتمالا
کار هر روز شما است، اما برای این فصل، ما روی موارد خاص از استخراج
زیرمسئله‌های غیرمرتبط تمرکز می‌کنیم، یعنی مواردی که کد استخراج شده
اطلاعای از چرایی فراخوان شدنش ندارد.

همان گونه که مشاهده خواهید کرد، هر چند در عمل این یک
تکنیک ساده است اما می‌تواند کد شما را به میزان قابل توجهی بهبود دهد.
ترفند اصلی این است که فعالانه، به دنبال زیرمسئله‌های غیرمرتبط بگردیم. با
این وجود، به دلایلی، بسیاری از برنامه‌نویسان به اندازه کافی از این تکنیک
استفاده نمی‌کنند.

در این فصل به مثال‌های مختلفی خواهیم پرداخت که اجرای این
تکنیک را در شرایط مختلفی که ممکن است در آن‌ها قرار بگیرید، نشان
می‌دهد.

## مثال مقدماتی[3]: findClosestLocation()

هدف سطح بالای کد `JavaScript` زیر،
پیدا کردن نزدیک‌ترین مکان به نقطه داده شده است(برای اینکه با هندسه
پیشرفته دچار سردرگمی نشوید، کد مربوط به آن را با به شکل italic
و `Bold` نشان داده‌ایم):

</div>

```
// Return which element of 'array' is closest to the given latitude/longitude.*
// Models the Earth as a perfect sphere.*

var findClosestLocation = function(lat, lng, array) {

    var closest;

    var closest_dist = Number.MAX_VALUE;

    for (var i = 0; i\ < array.length; i += 1) {
        // Convert both points to radians.*

        var lat_rad = radians(lat);

        var lng_rad = radians(lng);

        var lat2_rad = radians(array\[i\].latitude);
        var lng2_rad = radians(array\[i\].longitude);
        // Use the "Spherical Law of Cosines" formula.*
        var dist = Math.acos(Math.sin(lat_rad)\ * Math.sin(lat2_rad) Math.cos(lat_rad)\ * Math.cos(lat2_rad) Math.cos(lng2_rad - lng_rad));

        if (dist\ < closest_dist) {
            closest = array\[i\];
            closest_dist = dist;

        }
    }
    return closest;

};
```

<div dir="rtl">

کد داخل حلقه درباره زیرمسئله غیرمرتبط بوده و به فاصله
کروی[4] بین دو نقطه `lat/long`

را محاسبه می‌کند. از آنجا که تعداد زیادی کد در این قسمت وجود
دارد، منطقی است که آن را به صورت یک تابع جداگانه به نام
`spherical_distance()` استخراج کنیم:

</div>

```
var spherical_distance = function(lat1, lng1, lat2, lng2) {

    var lat1_rad = radians(lat1);

    var lng1_rad = radians(lng1);

    var lat2_rad = radians(lat2);

    var lng2_rad = radians(lng2);

    * // Use the "Spherical Law of Cosines" formula.*

    return Math.acos(Math.sin(lat1_rad)\ * Math.sin(lat2_rad) +

        Math.cos(lat1_rad)\ * Math.cos(lat2_rad)\ *

        Math.cos(lng2_rad - lng1_rad));

};
```

<div dir="rtl">

حال باقیمانده کد به این شکل می‌شود:
</div>

```
var findClosestLocation = function(lat, lng, array) {

    var closest;

    var closest_dist = Number.MAX_VALUE;

    for (var i = 0; i\ < array.length; i += 1) {

        var dist = spherical_distance(lat, lng, array\[i\].latitude, array\[i\].longitude);

        if (dist\ < closest_dist) {

            closest = array\[i\];

            closest_dist = dist;

        }

    }

    return closest;

};
```

<div dir="rtl">

این کد خوانایی بیشتری دارد زیرا خواننده می‌تواند بدون
اینکه حواسش به معادلات هندسی[5] پرت شود، بر روی
هدف سطح بالای کد تمرکز کند.

امتیاز دیگر این کد این است که
`spherical_distance()` برای تست ساده‌تر می‌باشد.
همچنین `spherical_distance()` تابعی است که می‌تواند
در آینده دوباره مورد استفاده قرار گیرد و به همین دلیل یک زیرمسئله
غیرمرتبط است. این تابع کاملا خود-مختار[6] بوده و
از نحوه استفاده برنامه‌ها از خود آگاهی ندارد.

## کدهایی با کاربرد خاص[7]

مجموعه‌ای از ابزارهای پایه وجود دارد که اکثر برنامه‌ها
از آن‌ها استفاده می‌کنند، مانند دستکاری رشته‌ها، استفاده از جداول
هش[8] و خواندن و نوشتن روی یک فایل.

اغلب این «ابزارهای پایه» توسط کتابخانه‌های
داخلی(built-in) به زبان برنامه‌نویسی شما پیاده
سازی شده‌اند. بعوان نمونه اگر بخواهید در `PHP` محتوای داخل یک فایل را بخوانید، می‌توانید تابع
`file_get_contents("filename")` را فراخوانی کنید و یا در
زبان `Python` می‌توانید از تابع
</div>

 `open("filename").read()`

<div dir="rtl">

استفاده کنید. اما زمان‌هایی نیز
هست که مجبورید خودتان دست به کار شده و برخی کمبودها را برطرف کنید. به
عنوان مثال در C++ هیچ روش مختصری برای خواندن کل
یک فایل وجود ندارد. بنابراین ناگزیر هستید که کدی شبیه این کد را
بنویسید:

</div>

```
ifstream file(file_name); 

// Calculate the file's size,  and allocate a buffer of that size.*

file.seekg(0,  ios::end); 

const int file_size = file.tellg(); 
char file_buf = new char [file_size]; 

// Read the entire file into the buffer.*

file.seekg(0,  ios::beg); 
file.read(file_buf,  file_size); 
file.close(); 

...
```

<div dir="rtl">

این یک مثال سنتی از یک زیرمسئله غیرمرتبط است که باید آن
را در یک تابع جدید مثل `ReadFileToString()` استخراج
کنید. اکنون بقیه کدپایه شما می‌تواند همانند زمانی عمل کند که انگار
زبان C++ از قبل تابع `ReadFileToString()` را داشته است.

به طور کلی هر گاه در حال فکر کردن به این جمله بودید که
«ای کاش کتابخانه ما یک تابع `XYZ()` داشت»، دست به
کار شده و آن را بنویسید! رفته رفته، مجموعه‌ای از کدهای سودمند به دست
خواهید آورد که می‌تواند در پروژه‌ها مختلف مورد استفاده قرار
گیرند.

## سایر کدهای همه منظوره[9]

هنگام اشکال‌زدایی در `JavaScript` اغلب از تابع alert() برای نمایش یک باکس
پیام استفاده می‌شود(یعنی همان نسخه وبِ تابعِ printf() برای اشکال‌زدایی) که برخی اطلاعات را به برنامه‌نویس نشان
می‌دهد. به عنوان مثال تابع زیر داده‌های ثبت شده را با استفاده از
Ajax به سرور فراخوانی می‌کند و سپس دیکشنری برگشت داده
شده از سرور را نشان می‌دهد:

</div>

```
ajax_post({

    url: 'http://example.com/submit',
    data: data, 
    on_success: function (response_data) {
        var str = "{"; 
        for (var key in response_data) {
            str += "  " + key + " = " + response_data[key] + "\n"; 
        }
        alert(str + "}"); 
         // Continue handling 'response_data' ...

    }

}); 
```

<div dir="rtl">

هدف سطح بالای این کد این است که یک فراخوانی Ajax
به سرور ایجاد کند و پاسخ آن را مدیریت کند. اما بسیاری از
کدها در حال حل زیرمسئله غیرمرتبط برای چاپِ زیبای[10] یک دیکشنری هستند. استخراج این کد در یک تابع جدید مانند
`format_pretty(obj)` کاری ساده است:
</div>

```
var format_pretty = function (obj) {

    var str = "{"; 
    for (var key in obj) {
        str += "  " + key + " = " + obj[key] + "\n"; 

    }

    return str + "}"; 

}; 
```

<div dir="rtl">

## مزایای غیر منتظره

دلایل زیادی وجود دارد که چرا استخراج تابع
`format_pretty()` ایده خوبی است. این فراخوانی کد را
ساده‌تر کرده و تابع `format_pretty()` را برای
استفاده در دیگر مکان‌ها نیز مفید می‌کند.

دلیل مهم‌تری نیز وجود دارد که به وضوح آشکار نیست:
**بهبود `format_pretty()` هنگامی که به صورت یک
تابع تکی است، کار ساده‌تری است وقتی که شما در
یک محیط ایزوله روی یک تابع کوچک‌تر کار می‌کنید،** اضافه کردن
ویژگی‌ها[11]، بهبود خوانایی کد، مراقبت از موارد
حاشیه‌ای و دیگر موارد، ساده‌تر به نظر می‌رسد.

در اینجا مواردی وجود دارد که `format_pretty(obj)`

توان مدیریت آن‌ها را ندارد:

* این تابع انتظار دارد که obj یک شئ[12] باشد. اگر به جای آن یک
    رشته ساده یا چیز نامشخصی باشد، کد فعلی یک استثناء[13] ایجاد می‌کند.

* این تابع انتظار دارد، هر مقداری از objیک نوع ساده باشد. اگر این شامل object‌های تودرتو باشد، کد تابع، آن‌ها را به صورت
    [object Object] نمایش خواهد داد که خیلی
    زیبا نیست.

قبل از اینکه format_pretty() را از تابع خود تفکیک کنیم، احساس می‌شود که انجام این بهبودها نیازمند کار زیادی باشد(در واقع، چاپ objectهای تودرتو به صورت بازگشتی آن هم بدون تابع جداگانه، کار بسیار دشواری است). اما بعد از تفکیک، اضافه کردن این قابلیت ساده خواهد بود. در ادامه کد بهبود یافته را مشاهده می‌کنید:
</div>

```
var format_pretty = function (obj,  indent) {

    // Handle null,  undefined,  strings,  and non-objects.

    if (obj === null) return "null"; 
    if (obj === undefined) return "undefined"; 
    if (typeof obj === "string") return '"' + obj + '"'; 
    if (typeof obj !== "object") return String(obj); 
    if (indent === undefined) indent = ""; 

    // Handle (non-null) objects.

    var str = "{\\n"; 

    for (var key in obj) {

        str += indent + "  " + key + " = "; 

        str += format_pretty(obj\[key\],  indent + "  ") + "\n"; 

    }

    return str + indent + "}"; 

}; 
```

<div dir="rtl">
این کد کاستی‌های ذکر شده را پوشش می‌دهد و خروجی زیر را
تولید می‌کند:
</div>

```
{

    key1 = 1

    key2 = true

    key3 = undefined

    key4 = null

    key5 = {

        key5a = {

            key5a1 = "hello world"

        }

    }

}
```

<div dir="rtl">

## ساختن کدهای همه منظوره[14] به تعداد زیاد

توابع ReadFileToString() و format_pretty() مثال‌های بسیار خوبی از زیرمسئله‌های غیرمرتبط هستند. آن‌ها به راحتی قابل استخراج بوده و می‌توانند در طول پروژه‌های مختلف، مجددا مورد استفاده قرار گیرند. کدپایه یک پروژه اغلب یک مسیر[15] ویژه برای کدهای همه منظوره دارد(مثلا پوشه util/) تا بتوان به راحتی آن‌ها را با دیگر پروژه‌ها به اشتراک گذاشت.

کدهای همه منظوره خیلی عالی هستند **زیرا به صورت کامل از بقیه پروژه شما جدا می‌شوند**. کدی مانند این، برای توسعه آسان‌تر، برای تست راحت‌تر و برای فهمیدن نیز ساده‌تر است.

به بسیاری از کتابخانه‌ها و سیستم‌های قدرتمند مورد استفاده خود، مانند دیتابیس‌های SQL، کتابخانه‌های JavaScript و سیستم‌های قالب‌بندی[16] HTML فکر کنید. لازم نیست نگران داخل آن‌ها باشید. این کدهای پایه از پروژه شما جداسازی شده‌اند و در نتیجه، کدپایه پروژه شما کوچک باقی می‌ماند.

هرچه بیشتر پروژه خود را به عنوان کتابخانه‌های جدا تفکیک کنید، بهتر خواهد بود. زیرا بقیه کد شما کوچک‌تر شده و فکر کردن درباره آن راحت‌تر خواهد بود.

## آیا این برنامه‌نویسی بالا-به-پایین[17] است یا پایین-به-بالا[18]؟

برنامه‌نویسی بالا-به-پایین سبکی است که در آن ماژول‌ها و توابعِ بالاترین-سطح، ابتدا طراحی می‌شوند و توابع سطح-پایین در صورت نیاز برای پشتیبانی از توابع سطح-بالا، پیاده سازی می‌شوند. برنامه‌نویسی پایین-به-بالا سعی دارد ابتدا همه زیرمسئله‌ها را پیش بینی و حل کند و سپس کامپوننت‌های سطح-بالا را با استفاده از این اجزاء بسازد. در این فصل از یک رویکرد در مقابل رویکرد دیگر دفاع نمی‌شود خصوصا که اکثر برنامه‌نویسی‌ها ترکیبی از هر دو رویکرد است. هدف اصلی این است که زیرمسئله‌ها حذف شده و به
صورت جداگانه حل شوند.

## قابلیت‌های پروژه-محور[19]

در حالت ایده‌آل، زیرمسئله‌ای که شما استخراج می‌کنید، به طور کامل project-agnostic[20] خواهد بود. حتی اگر اینگونه نباشد، باز هم اشکالی ندارد، شکستن زیرمسئله‌ها هنوز هم باعث شگفتی می‌شود.

در اینجا مثالی از یک وبسایت بررسی کسب و کار، آورده شده است. این کد Python، ابتدا یک شئ Business ایجاد می‌کند و برای آن name، url و date_created را تنظیم می‌کند:
</div>

```
business = Business()
business.name = request. POST["name"]
url_path_name = business.name.lower()
url_path_name = re.sub(r"\['\\.\]",  "",  url_path_name)
url_path_name = re.sub(r"\[^a-z0-9\]+",  "-",  url_path_name)
url_path_name = url_path_name.strip("-")
business.url = "/biz/" + url_path_name
business.date_created = datetime.datetime.utcnow()
business.save_to_database()
```

<div dir="rtl">

قرار است url نسخه تمیزی ازname باشد. به عنوان مثال اگر name برابر A. C. Joe’s Tire & Smog, Inc. باشد، url به شکل `/biz/ac-joes-tire-smog-inc` خواهد بود. زیرمسئله غیرمرتبط در این کد «**قرار دادن یک نام داخل یک URL معتبر»** است. ما می‌توانیم این کد را سریع و راحت استخراج کنیم و در حالی که در این مرحله هستیم عبارات منظم[21] را از قبل کامپایل نموده و به آن‌ها اسامی قابل خواندن بدهیم:
</div>

```
CHARS_TO_REMOVE = re.compile(r"\['\\.\]+")

CHARS_TO_DASH = re.compile(r"\[^a-z0-9\]+")

def make_url_friendly(text):
    text = text.lower()
    text = CHARS_TO_REMOVE.sub('',  text)
    text = CHARS_TO_DASH.sub('-',  text)
    return text.strip("-")

```

<div dir="rtl">

اکنون کد اصلی الگوی «منظم‌تری[22]» دارد:
</div>

```
business = Business()
business.name = request. POST\["name"\]
business.url = "/biz/" + make_url_friendly(business.name)
business.date_created = datetime.datetime.utcnow()
business.save_to_database()
```

<div dir="rtl">

این کد برای خوانده شدن به تلاش کمتری نیاز دارد، زیرا با  وجود عبارات منظم و دستکاری رشته‌های عمیق[23]، دچار ابهام نمی‌شوید.

حال این سوال مطرح است که باید کد مربوط به `make_url_friendly()` را کجا قرار دهید؟ از آنجا که به نظر می‌رسد یک تابع نسبتا کلی است، بنابراین شاید قرار دادن آن در مسیرِ جداگانه `util/` منطقی به نظر برسد اما از سوی دیگر، این عبارات منظم با نام تجاری U. S طراحی شده‌اند، بنابراین شاید این کد باید در همان فایلی که استفاده شده است باقی بماند.
این واقعا مهم نیست که حجم آن زیاد باشد، به راحتی می‌توانید تعریف آن را در آینده تغییر دهید. نکته مهم‌تر این است که `make_url_friendly()` به هیچ وجه استخراج نشده بود.

## ساده سازی Interface موجود

همه افراد، کتابخانه‌ای که یک interface واضح و تمیز ارائه می‌دهد را دوست دارند. مواردی که آرگومان‌های کمی گرفته، تنظیمات راه اندازی زیادی نداشته و به طور کلی تلاش کمی برای استفاده از آن‌ها نیاز است. این باعث می‌شود کد شما زیبا به نظر برسد: هم زمان ساده و قدرتمند.

اما اگر interface‌ مورد استفاده واضح نبود، هنوز هم می‌توانید توابع wrapper[24] خود را که واضح هستند، بسازید.

به عنوان مثال، کار با کوکی‌های[25] مرورگر در JavaScript خیلی ایده آل نیست. از نظر مفهومی، کوکی‌ها مجموعه‌ای از جفت‌های `name/vale` هستند. اما interface فراهم شده توسط مرورگر یک رشته تکی `document.cookie` را ارائه می‌دهد که syntax آن به شکل زیر است:
</div>

```
name1=value1;  name2=value2;  ...
```

<div dir="rtl">

برای پیدا کردن کوکی مورد نظر مجبور به تجزیه این رشته غول پیکر هستید. در اینجا کدی داریم که مقدار کوکی با نام `max_results` را می‌خواند:
</div>

```
var max_results; 

var cookies = document.cookie.split('; '); 
for (var i = 0;  i \< cookies.length;  i++) {
    var c = cookies\[i\]; 
    c = c.replace(/^\[ \]+/,  '');   *// remove leading spaces*
    if (c.indexOf("max_results=") === 0)
        max_results = Number(c.substring(12,  c.length)); 

}
```

<div dir="rtl">

ظاهرا این کد خیلی زشت به نظر می‌رسد. همانطور که انتظار می‌رود، تابع `get_cookie()` از قبل نوشته شده است، بنابراین ما می‌توانیم فقط بنویسیم:
</div>

```
var max_results = Number(get_cookie("max_results")); 
```

<div dir="rtl">

عجیب‌تر از این، ساختن یا تغییر دادن مقدار یک کوکی است. شما مجبورید `document.cookie` را با یک مقدار دقیقا به شکل زیر تنظیم کنید:
</div>

```
document.cookie = "max_results=50;  expires=Wed,  1 Jan 2020 20:53:47 UTC;  path=/";
```

<div dir="rtl">

ظاهرا باید این دستور تمام کوکی‌های موجود دیگر را نیز بازنویسی کند، اما این کار را انجام نمی‌دهد. یک interface ایده‌آل‌تر برای تنظیم کوکی چیزی شبیه این خواهد بود:
</div>

```
set_cookie(name,  value,  days_to_expire); 
```

<div dir="rtl">

پاک کردن یک کوکی همچنان کاری غیر معمول است چرا که باید از قبل برای آن زمان انقضاء تعیین کنید. در عوض یک interface ایده‌آل به سادگی به صورت زیر خواهد بود:
</div>

```
delete_cookie(name); 
```

<div dir="rtl">

موضوع اصلی اینجا این است که **شما هرگز نباید به یک interface غیر ایده‌ال راضی شوید.** شما همواره می‌توانید توابع wrapper خود را برای مخفی کردن جزئیات زشت interface‌هایی که در آن گیر افتاده‌اید، ایجاد کنید.

## تغییر شکل[26] مجدد Interface با توجه به نیاز

کدهای زیادی در یک برنامه فقط برای پشتیبانی از کدهای دیگر استفاده می‌شوند همچون تنظیم ورودی‌ها برای یک تابع یا ارسال خروجی‌های پردازش شده. این کد «چسبان[27]» معمولا هیچ ارتباطی با منطق اصلی برنامه شما ندارد. کدهای بدون تغییری مانند این، گزینه‌ای عالی برای دریافت[28] از توابع، بصورت جداگانه هستند.

به عنوان مثال، فرض کنید یک دیکشنری Python که شامل اطلاعات حساس کاربر مانند `{"username": "...", "password": "..."}` داشته و باید همه این اطلاعات را در URL قرار دهید. به دلیل حساس بودن این اطلاعات، تصمیم می‌گیرید که ابتدا دیکشنری را با استفاده از یک کلاس `Cipher`

رمزنگاری کنید.

اما باید توجه کنید که Cipher از یک سو انتظار دارد که رشته‌ای از بایت‌ها[29](نه یک دیکشنری) را به عنوان ورودی دریافت کند و از سوی دیگر نیز یک رشته از بایت‌ها را بر می‌گرداند، اما چیزی که ما نیاز داریم یک URL امن است. `Cipher` همچنین تعدادی پارامتر اضافی را گرفته و برای استفاده خیلی دست و پا گیر است.

آنچه به عنوان یک کار ساده شروع شده بود به کد چسبان[30] بزرگی تبدیل می‌شود:
</div>

```
user_info = { "username": "...",  "password": "..." }
user_str = json.dumps(user_info)

cipher = Cipher("aes_128_cbc",  key=PRIVATE_KEY,  init_vector=INIT_VECTOR,  op=ENCODE)

encrypted_bytes = cipher.update(user_str)
encrypted_bytes += cipher.final()  # flush out the current 128 bit block

url = "http://example.com/?user_info=" + base64.urlsafe_b64encode(encrypted_bytes)

...
```

<div dir="rtl">

حتی اگر بتوانیم مشکل را با رمزنگاری اطلاعات کاربر داخل یک URL حل کنیم، باز قسمت اعظم کد فقط در حال رمزنگاری این شئ Python در یک رشته URL-frindly است. راه حل ساده استخراج زیرمسئله است:
</div>

```
def url_safe_encrypt(obj):

    obj_str = json.dumps(obj)
    cipher = Cipher("aes_128_cbc",  key=PRIVATE_KEY,  init_vector=INIT_VECTOR,  op=ENCODE)
    encrypted_bytes = cipher.update(obj_str)

    encrypted_bytes += cipher.final()  # flush out the current 128 bit block

    return base64.urlsafe_b64encode(encrypted_bytes)
```

<div dir="rtl">

حال نتیجه کد برای اجرای منطق واقعی برنامه، ساده می‌شود:
</div>

```
user_info = { "username": "...",  "password": "..." }

url = "http://example.com/?user_info=" + url_safe_encrypt(user_info)
```

<div dir="rtl">

## دور نگه داشتن بیش از حد توابع از یکدیگر

همان گونه که در ابتدای این فصل گفتیم، هدف ما پشتکار[31] بیشتر برای شناسایی و استخراج زیرمسئله‌های غیرمرتبط است. ما می‌گوییم پشتکار، زیرا اکثر کدنویسان بهاندازه کافی پرتلاش نیستند. اما گاهی این امکان وجود دارد که چیزها را بیش از حد جداسازی کنید.

به عنوان مثال، کد بخش قبلی می‌توانست به موارد بیشتری مانند کد زیر شکسته شود:
</div>

```
user_info = { "username": "...",  "password": "..." }

url = "http://example.com/?user_info=" + url_safe_encrypt_obj(user_info)

def url_safe_encrypt_obj(obj):
    obj_str = json.dumps(obj)
    return url_safe_encrypt_str(obj_str)

def url_safe_encrypt_str(data):
    encrypted_bytes = encrypt(data)
    return base64.urlsafe_b64encode(encrypted_bytes)

def encrypt(data):
    cipher = make_cipher()
    encrypted_bytes = cipher.update(data)
    encrypted_bytes += cipher.final()     # flush out any remaining bytes
    return encrypted_bytes

def make_cipher():

  return Cipher("aes_128_cbc",  key=PRIVATE_KEY,  init_vector=INIT_VECTOR,  op=ENCODE)
```

<div dir="rtl">

بی شک معرفی همه این توابع کوچک، به خوانایی کد آسیب
می‌زند، زیرا خواننده باید حضور ذهن بیشتری داشته باشد، خصوصا که دنبال کردن مسیر اجرایی، نیازمند پرش به اطراف است. ولی به هر حال اضافه کردن توابع جدید شامل یک هزینه خوانایی کوچک(اما ملموس) است که باید بپردازید و اندکی از خوانایی کد صرف نظر کنید.

## خلاصه فصل

هدف اصلی این فصل ارائه روشی ساده یعنی **جدا کردن کد عمومی از کدهای ویژه پروژه است**. از آنجا که بیشتر قسمت‌های کد، عمومی است، برای حل مشکلات کلی می‌توان یک مجموعه بزرگ از کتابخانه‌ها و توابع کمکی را ایجاد نمود و آنچه که باقی می‌ماند یک هسته کوچک خواهد بود که برنامه شما را منحصر به فرد می‌نماید.

دلیل اصلی مفید بودن این تکنیک این است که به برنامه‌نویسان اجازه می‌دهد تا روی مشکلات کوچک‌تر که به خوبی تعریف شده و از بقیه پروژه جدا شده‌اند تمرکز کنند. در نتیجه، راه حل‌های بهتر و صحیح‌تری برای این زیرمسئله‌ها پیدا خواهید شد. مزیت آخر نیزاینکه امکان استفاده مجدد از آن‌ها در آینده وجود دارد.

## پیشنهاد مطالعه بیشتر

در کتاب **Refactoring** از **Martin Fowler** روشی برای بهبود طراحی کد موجود در «**متد استخراجی**» با عنوان بازسازی توصیف شده است و همچنین بسیاری از روش‌های دیگر، برای بازسازی کد در این کتاب ارائه شده است.

**Kent Beck** در کتاب **Smalltalk Best Practice Patterns** الگوی متد ترکیبی را شرح می‌دهد و شامل تعدادی از اصول مربوط به شکستن کد به توابع کوچک است. به خصوص، یکی از اصل‌ها این است: همه عملیات‌ها[32] را در یک متد تکی در همان سطح انتزاع[33] نگهداری کنید.

این ایده‌ها، مشابه توصیه ما در مورد استخراج زیرمسئله‌های غیرمرتبط است. آنچه در این فصل مورد بحث قرار دادیم، یک مورد ساده و خاص، از استخراج یک متد است.
</div>

[1] Subproblems
[2] high-level
[3] Introductory
[4] spherical
[5] geometry
[6] self-contained
[7] Pure Utility Code
[8] hash tables
[9] General-Purpose
[10] Pretty-print
[11] features
[12] object
[13] exception
[14] General-Purpose
[15] directory
[16] templateing
[17] TOP-DOWN
[18] BOTTOM-UP
[19] Project-Specific Functionality

<div dir="rtl">

[20] به چیزی اطلاق می‌شود که بتواند تعمیم یابد یا به
گونه‌ای در بین سیستم‌های مختلف قابل سازگار باشد.

</div>

[21] regular expressions
[22] regular
[23] deep string

<div dir="rtl">

[24] یک تابع wrapper یک زیرروال
در یک کتابخانه نرم افزار و یا یک برنامه کامپیوتری است که هدف اصلی آن
فراخوانی یک زیرروال دوم و یا یک فراخوان سیستمی با کمی و یا بدون محاسبات
اضافی است.
</div>

[25] cookie
[26] Reshaping
[27] glue
[28] pull
[29] string of bytes
[30] glue code
[31] aggressive
[32] operations
[33] abstraction
