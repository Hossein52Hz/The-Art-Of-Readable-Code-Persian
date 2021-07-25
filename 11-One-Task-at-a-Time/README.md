
<div dir='rtl'>

# فصل یازدهم در هر لحظه یک وظیفه

</div>

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/11-One-Task-at-a-Time/img-11-1.png" />
</p>

<div dir='rtl'>

درک کردن کدی که چندین کار را به شکل همزمان انجام می‌دهد، سخت‌تر است. یک بلوک تکی کد ممکن است اشیای جدید1، پالایش داده2، تجزیه ورودی‌ها3 و اعمال منطق کسب و کار را به صورت هم زمان آماده سازی4 کند. درک کل این کدهای در کنار هم بافته شده نسبت به زمانی که هر وظیفه5 به تنهایی شروع و تکمیل شود، سخت‌تر است.

> **_کلید طلایی:_**  کد باید به گونه‌ای سازماندهی شده باشد که در یک لحظه فقط یک وظیفه را انجام دهد.

به بیان دیگر، این فصل درباره «یکپارچه‌سازی1» کدهای شما است. نمودار زیر، این فرآیند را نشان می‌دهد. سمت چپ تصویر وظایف مختلف انجام شده توسط یک قطعه کد را نشان داده و سمت راست تصویر، همان کد را، بعد از سازماندهی آن برای انجام هر وظیفه در یک لحظه، نشان می‌دهد.

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/11-One-Task-at-a-Time/img-11-2.png" />
</p>

شاید این توصیه را شنیده باشید که: توابع فقط باید یک کار را انجام دهند. توصیه ما نیز شبیه همین است، چرا که همیشه درباره مرزهای1 تابع صدق نمی‌کند. مطمئنا شکستن یک تابع بزرگ به چندین تابع کوچک می‌تواند خوب باشد. اما اگر این کار را هم نکنید، هنوز می‌توانید کد را در داخل همان تابع بزرگ نیز سازمان دهی کرده و این احساس را ایجاد کنید که بخش‌های منطقی، از هم جدا شده‌اند.

فرآیند زیر کاری است که ما برای ساختن «کدی که در یک لحظه فقط یک وظیفه را انجام می‌دهد» استفاده می‌کنیم: 

1. تمام وظایفی که کد شما انجام می‌دهد را لیست کنید. منظور ما از وظیفه(task) معنایی بسیار گسترده است که می‌تواند به کوچکی این جمله: «اطمینان حاصل کنید که این object معتبر است» یا به مبهمی این جمله: «از طریق تکرار هر گره 2در درخت» باشد.
2. سعی کنید این وظایف را تا حد ممکن در توابع مختلف جداسازی کرده و حداقل در بخش‌های مختلف کد تقسیم کنید. 

## وظیفه‌ها می‌توانند کوچک باشند

فرض کنید یک ابزارک رای گیری در یک وبلاگ وجود دارد تا کاربر بتواند به یک کامنت، امتیاز مثبت(Up) یا منفی(Down) بدهد. مجموع امتیاز برای یک کامنت به این شکل محاسبه می‌شود که برای هر رای مثبت +1 و برای هر رای منفی -1 امتیاز تعلق می‌گیرد. 

در اینجا سه حالت برای یک رای کاربر و اینکه چگونه می‌تواند بر امتیاز کل اثر بگذارد وجود دارد:

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/11-One-Task-at-a-Time/img-11-3.png" />
</p>

زمانی که کاربر روی دکمه کلیک می‌کند(برای رای دادن یا تغییر رای) کد JavaScript زیر صدا زده می‌شود:

</div>

```

vote_changed(old_vote, new_vote);  // each vote is "Up", "Down", or ""

```
<div dir='rtl'>

این تابع مجموع امتیاز را به‌روزرسانی می‌کند و برای همه ترکیب‌های old_vote/new_vote اجرا می‌شود:

</div>

```

var vote_changed = function (old_vote, new_vote) {
    var score = get_score();
    if (new_vote !== old_vote) {
        if (new_vote === 'Up') {
            score += (old_vote === 'Down' ? 2 : 1);
        } else if (new_vote === 'Down') {
            score -= (old_vote === 'Up' ? 2 : 1);
        } else if (new_vote === '') {
            score += (old_vote === 'Up' ? -1 : 1);
        }
    }
    set_score(score);
};

```
<div dir='rtl'>

اگرچه کد از نظر کوتاهی خیلی خوب است، اما کارهای زیادی را انجام می‌دهد. جزئیات پیچده‌ای وجود داشته و این دشوار است که با یک نگاه اجمالی بگویید، آیا خطاهای غیرمترقبه، خطاهای تایپوگرافی1 یا باگ‌های دیگر وجود دارد یا نه؟ ظاهرا این کد تنها یک کار (یعنی به‌روزرسانی امتیاز) را انجام می‌دهد اما در واقع دو وظیفه وجود دارد که به صورت هم‌زمان در حال انجام است:

1. گزینه old_vote و new_vote به مقادیر عددی تبدیل2 می‌شوند.
2. امتیاز به‌روزرسانی می‌شود.

ما می‌توانیم با انجام هر وظیفه به صورت جداگانه، خواندن کد را آسانتر کنیم. کد زیر اولین وظیفه برای تبدیل رای به یک مقدار عددی را حل می‌کند:

</div>

```

var vote_value = function (vote) {
    if (vote === 'Up') {
        return +1;
    }
    if (vote === 'Down') {
        return -1;
    }
    return 0;
};

```
<div dir='rtl'>

حال بقیه کد می‌تواند وظیفه دوم، یعنی به‌روزرسانی امتیاز را حل کند:

</div>

```

var vote_changed = function (old_vote, new_vote) {
    var score = get_score();
    score -= vote_value(old_vote);  // remove the old vote
    score += vote_value(new_vote);  // add the new vote
    set_score(score);
};

```
<div dir='rtl'>

همان گونه که می‌بینید: این نسخه از کد، تلاش ذهنی کمتری برای متقاعد کردن شما در این مورد که این کد کار می‌کند، نیاز دارد. این نکته‌ای مهم در مورد دلایل آسان شدن درک کد است.

## استخراج مقدارها از یک Object

زمانی ما یک کد JavaScript داشتیم که مکان یک کاربر را در یک رشته از City, Country قرار می‌داد. مانند Santa Monica, USA یا Paris, France. در واقع ما یک دیکشنری location_info با اطلاعات ساخت‌یافته فراوان داشتیم. تنها کاری که باید انجام می‌دادیم این بود که، یک شهر و یک کشور را از همه فیلدها انتخاب و سپس آن‌ها را به هم الحاق کنیم. تصویر زیر مثالی از ورودی و خروجی این کد را نشان می‌دهد:

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/11-One-Task-at-a-Time/img-11-4.png" />
</p>

ابتدا به نظر می‌رسد که با چیز ساده‌ای روبرو هستیم، اما قسمت مشکل این است که برخی یا همه این چهار مقدار، ممکن است وجود نداشته باشند. در اینجا نحوه برخورد با آن‌ها آورده شده است:

* هنگام انتخاب شهر، ما ترجیح می‌دادیم در صورت وجود، ابتدا از LocalityName که همان (city/town) است، استفاده کنیم و سپس از SubAdministrativeAreaName یا همان(larger city/county) و بعد از آن از AdministrativeAreaName یا همان(state/territory) استفاده کنیم.
* اگر هر سه مورد وجود نداشته باشند، نام شهر، با توجه به مقدار پیش‌فرض MiddleofNowhere تعیین می‌شد.
* اگر نام CountryName وجود نداشته باشد، عبارت Planet Earth به عنوان پیش‌فرض انتخاب می‌شد. 

تصویر زیر دو نمونه از مدیریت مقدارهای از دست رفته یا ناموجود را نشان می‌دهد:

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/11-One-Task-at-a-Time/img-11-5.png" />
</p>

در ادامه کدی را که برای پیاده سازی این وظیفه نوشته‌ایم، آورده شده است:

</div>

```

var place = location_info["LocalityName"];  // e.g. "Santa Monica"
if (!place) {
    place = location_info["SubAdministrativeAreaName"];  // e.g. "Los Angeles"
}
if (!place) {
    place = location_info["AdministrativeAreaName"];  // e.g. "California"
}
if (!place) {
    place = "Middle-of-Nowhere";
}
if (location_info["CountryName"]) {
    place += ", " + location_info["CountryName"];  // e.g. "USA"
} else {
    place += ", Planet Earth";
}
return place;

```
<div dir='rtl'>

مطمئنا این کمی کثیف است اما کار مد نظر ما را انجام می‌داد. 

چند روز بعد ما نیازمند بهبود این عملکرد شدیم چرا که می‌خواستیم برای مکان‌هایی در United States، به جای نام کشور(county) نام ایالت(state) را در صورت وجود، نمایش دهیم و در نتیجه به جای Santa Monica, USA مقدار Santa Monica, California بازگردانده می‌شد. بی شک افزودن این ویژگی به کد قبلی سبب زشت‌تر شدن آن خواهد شد. 

## اعمال کردن «یک وظیفه در یک لحظه»

به جای اعمال فشار برای تغییر این کد به چیزی که می‌خواستیم، کمی درنگ کرده و متوجه شدیم که این کد، قبلا هم چندین وظیفه را به طور هم‌زمان انجام می‌داده است:

1. استخراج مقدارها از دیکشنری location_info
2. City را از طریق ترتیب اولویت به دست آورید، در صورت پیدا نکردن چیزی، مقدار پیش‌فرض را برابر Middle-of-Nowhere قرار دهید.
3. به دست آوردن Country و در صورتی که وجود نداشت از مقدار Planet Earth استفاده کنید.
4. مکان را به‌روزرسانی کنید

نتیجه این که، ما کد اصلی را بازنویسی خواهیم کرد تا هر یک از این وظیفه‌ها به صورت مستقل حل شوند. اولین وظیفه یعنی استخراج مقدارها از location_info به راحتی قابل حل است:

</div>

```

var town    = location_info["LocalityName"];               // e.g. "Santa Monica"
var city    = location_info["SubAdministrativeAreaName"];  // e.g. "Los Angeles"
var state   = location_info["AdministrativeAreaName"];     // e.g. "CA"
var country = location_info["CountryName"];                // e.g. "USA"

```
<div dir='rtl'>

در این مرحله، کار ما با استفاده از location_info انجام شده است و لازم نیست آن کلیدهای1 طولانی را به خاطر بسپاریم. در عوض، چهار متغیر ساده برای کار با آن‌ها داریم. در مرحله بعد، باید بفهمیم که مقدار برگشتی second_half  چه چیزی باید باشد:

</div>

```

// Start with the default, and keep overwriting with the most specific value.
var second_half = "Planet Earth";
if (country) {
    second_half = country;
}
if (state && country === "USA") {
    second_half = state;
}

```
<div dir='rtl'>

به طور مشابه، می‌توانیم مقدار first_half  را نیز بدانیم:

</div>

```

var first_half = "Middle-of-Nowhere";
if (state && country !== "USA") {
    first_half = state;
}
if (city) {
    first_half = city;
}
if (town) {
    first_half = town;
}

```
<div dir='rtl'>

و در نهایت ما اطلاعات را به یکدیگر می‌چسبانیم:

</div>

```

return first_half + ", " + second_half;

```
<div dir='rtl'>

تصویر یکپارچه سازی1 در ابتدای این فصل، در واقع یک نمایش مجدد از راه حل اصلی و نسخه جدید بود. در اینجا همان تصویر با جزئیات بیشتر ارائه شده است:

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/11-One-Task-at-a-Time/img-11-6.png" />
</p>

همان گونه که می‌بینید، در راه حل دوم چهار وظیفه به مناطق مجزا تقسیم شده است.

## یک رویکرد دیگر

هنگام بازسازی1 کد، اغلب چندین راه وجود دارد و این مورد نیز از این قاعده مستثنی نیست.

هنگامی که برخی از این وظایف را جدا کردید، فکر کردن در مورد کد راحت‌تر شده و ممکن است روش‌های بهتری برای بازسازی مجدد پیدا کنید. به عنوان نمونه، برای درک کردن مجموعه دستورات if اخیر، به دقت بیشتری برای خواندن کد نیاز داریم که آیا هر مورد به درستی کار می‌کند یا نه؟ در واقع دو زیروظیفه2 به صورت هم‌زمان در کد وجود دارد:

لیستی از متغیرها را مرور کرده و در صورت وجود یکی را که ارجحیت بیشتری دارد، انتخاب کنید. 

بسته به اینکه کشور USA باشد، یک لیست متفاوت انتخاب کنید.

با نگاه دوباره به قبل، می‌توانید ببینید که کد دارای منطق «if USA» با بقیه منطق کد، به هم آمیخته شده است. در عوض، می‌توانیم موارد USA و غیر USA را به صورت جداگانه مدیریت کنیم:

</div>

```

var first_half, second_half;
if (country === "USA") {
    first_half = town || city || "Middle-of-Nowhere";
    second_half = state || "USA";
} else {
    first_half = town || city || state || "Middle-of-Nowhere";
    second_half = country || "Planet Earth";
}
return first_half + ", " + second_half;

```
<div dir='rtl'>

در صورتی که با JavaScript آشنا نیستید، باید بدانید که عبارت a || b || c یک عبارت اتمیک است و با اولین مقدار true، ارزیابی خاتمه می‌یابد(در این مورد، رشته تعریف شده، یک رشته تهی1 نیست). مزیتی که این کد دارد این است که، بررسی کردن لیست‌های برگزیده و به‌روزرسانی آن‌ها را بسیار ساده کرده است. همچنین بیشتر دستورات if حذف شده‌اند و منطق تجاری مجددا توسط خطوط کمتری، پیاده‌سازی شده است.

## یک مثال بزرگتر

در یک سیستم خزیدن در وب2 که ساخته بودیم، یک تابع با نام UpdateCounts() برای افزایش آمار مختلف هر صفحه وبِ دانلود شده، فراخوانی می‌شد:

</div>

```

void UpdateCounts(HttpDownload hd) {
    counts["Exit State"   ][hd.exit_state()]++;      // e.g. "SUCCESS" or "FAILURE"
    counts["Http Response"][hd.http_response()]++;   // e.g. "404 NOT FOUND"
    counts["Content-Type" ][hd.content_type()]++;    // e.g. "text/html"
}

```
<div dir='rtl'>

خب، این همان چیزی است که می‌خواستیم، کد به نظر برسد!

در واقع، شئ HttpDownload هیچ یک از متدهای نشان داده شده را نداشت. در عوض، HttpDownload یک کلاس خیلی بزرگ و پیچیده، با تعداد زیادی کلاس‌های تودرتو بود و مجبور بودیم خودمان این مقدارها را بیرون بکشیم. گاهی اوقات این مقدارها کاملا از بین رفته و اوضاع وخیم‌تر می‌شد، در این حالت از عبارت unknown به عنوان مقدار پیش‌فرض استفاده می‌کردیم. 

به این دلایل، کد واقعی کاملا آشفته بود:

</div>

```

// WARNING: DO NOT STARE DIRECTLY AT THIS CODE FOR EXTENDED PERIODS OF TIME.
void UpdateCounts(HttpDownload hd) {
    // Figure out the Exit State, if available.
    if (!hd.has_event_log() || !hd.event_log().has_exit_state()) {
        counts["Exit State"]["unknown"]++;
    } else {
        string state_str = ExitStateTypeName(hd.event_log().exit_state());
        counts["Exit State"][state_str]++;
    }
  // If there are no HTTP headers at all, use "unknown" for the remaining elements.
    if (!hd.has_http_headers()) {
        counts["Http Response"]["unknown"]++;
        counts["Content-Type"]["unknown"]++;
        return;
    }
    HttpHeaders headers = hd.http_headers();
    // Log the HTTP response, if known, otherwise log "unknown"
    if (!headers.has_response_code()) {
        counts["Http Response"]["unknown"]++;
    } else {
        string code = StringPrintf("%d", headers.response_code());
        counts["Http Response"][code]++;
    }
    // Log the Content-Type if known, otherwise log "unknown"
    if (!headers.has_content_type()) {
        counts["Content-Type"]["unknown"]++;
    } else {
        string content_type = ContentTypeMime(headers.content_type());
        counts["Content-Type"][content_type]++;
    }
}

```
<div dir='rtl'>

همان گونه که مشاهده می‌کنید، در اینجا خطوط کد بسیار زیاد و تعداد زیادی منطق و حتی چند خط تکراری از کد وجود دارد. این کد برای خواندن جالب نیست. 

اشکال اساسی این کد این است که بین وظیفه‌های مختلف، عقب و جلو می‌رود. در اینجا وظایف مختلفی وجود دارد که در کل کد در هم تنیده شده‌اند:

1. استفاده از unknown به عنوان مقدار پیش‌فرض برای هر key
2. تشخیص اینکه اعضای HttpDownload از دست رفته است یا نه.
3. استخراج مقدار و تبدیل آن به یک رشته.
4. به‌روزرسانی counts[ ].

می توانیم با جداسازی برخی از این وظیفه‌ها به مناطق مجزا، کد را بهبود دهیم:

</div>

```

void UpdateCounts(HttpDownload hd) {
    // Task: define default values for each of the values we want to extract
    string exit_state = "unknown";
    string http_response = "unknown";
    string content_type = "unknown";
    // Task: try to extract each value from HttpDownload, one by one
    if (hd.has_event_log() && hd.event_log().has_exit_state()) {
        exit_state = ExitStateTypeName(hd.event_log().exit_state());
    }
    if (hd.has_http_headers() && hd.http_headers().has_response_code()) {
        http_response = StringPrintf("%d", hd.http_headers().response_code());
    }
    if (hd.has_http_headers() && hd.http_headers().has_content_type()) {
        content_type = ContentTypeMime(hd.http_headers().content_type());
    }
    // Task: update counts[]
    counts["Exit State"][exit_state]++;
    counts["Http Response"][http_response]++;
    counts["Content-Type"][content_type]++;
}

```
<div dir='rtl'>

همان گونه که مشاهده می‌کنید، کد دارای سه منطقه جداگانه با اهداف زیر است:

1. تعریف پیش‌فرض‌ها برای سه کلید مورد نظر ما.
2. استخراج مقدارها (در صورت موجود بودن) برای هر یک از این کلیدها و تبدیل آن‌ها به رشته.
3. به‌روزرسانی count[ ] برای هر کلید/مقدار (key/value)

مزیت این مناطق این است که آن‌ها از یکدیگر جداسازی شده‌اند و زمانی که در حال خواندن یک منطقه هستید، لازم نیست در مورد دیگر مناطق فکر کنید.

توجه داشته باشید که اگرچه ما چهار وظیفه را لیست کرده بودیم ولی تنها قادر به جداسازی سه مورد از آن‌ها شدیم. این کاملا خوب است، در واقع وظیفه‌هایی که در ابتدا لیست می‌کنید، فقط یک نقطه شروع بوده و حتی جدا کردن برخی از آن‌ها (و نه همه آن‌ها) می‌تواند کمک زیادی کند.

## بهبودهای بیشتر1 

این نسخه جدید کد بهبود قابل توجهی نسبت به هیولای اولیه دارد و ما حتی در انجام این پاک‌سازی نیازی به ایجاد توابع دیگری نداشتیم. همان گونه که قبلا اشاره کردیم، ایده «یک وظیفه در هر لحظه» می‌تواند به شما برای پاکسازی کد، صرف نظر از مرزهای توابع کمک کند. 

با این حال، ما همچنان می‌توانستیم این کد را با روش دیگری، یعنی با معرفی سه تابع کمکی بهبود بخشیم:

</div>

```

void UpdateCounts(HttpDownload hd) {
    counts["Exit State"][ExitState(hd)]++;
    counts["Http Response"][HttpResponse(hd)]++;
    counts["Content-Type"][ContentType(hd)]++;
}

```
<div dir='rtl'>

این توابع مقدار متناظر را استخراج کرده و یا مقدار unknown را بر می‌گردانند. به عنوان مثال:

</div>

```

string ExitState(HttpDownload hd) {
    if (hd.has_event_log() && hd.event_log().has_exit_state()) {
        return ExitStateTypeName(hd.event_log().exit_state());
    } else {
        return "unknown";
    }
}

```
<div dir='rtl'>

توجه داشته باشید که این راه حل جایگزین، حتی هیچ متغیری را تعریف نمی‌کند! همان گونه که در فصل نهم، قسمت متغیرها و خوانایی اشاره کردیم، می‌توان متغیرهایی که نتایج واسط (یا موقت) را نگه‌داری می‌کنند، به طور کامل حذف کرد. 
در این راه حل، به سادگی مشکل را در جهت دیگری تکه تکه کرده‌ایم. هر دو راه حل خوانایی قابل توجهی دارند و خواننده هنگام خواندن کد تنها به یک وظیفه در یک زمان فکر می‌کند.

## خلاصه فصل

در این فصل یک تکنیک ساده برای سازماندهی کد یعنی **«تنها یک وظیفه را در یک زمان انجام دهید»** ارائه گردید. 

شاید برخی از این وظیفه‌ها به سادگی به توابع یا کلاس‌های جداگانه تبدیل شوند. بقیه ممکن است فقط یک پاراگراف منطقی در یک تابع تکی باشند. جزئیات دقیقِ چگونگیِ جداسازیِ این وظیفه‌ها به اندازه واقعیت جدا شدن آن‌ها مهم نیست بلکه قسمت سخت این کار توصیف دقیق همه کارهای کوچکی است که برنامه شما انجام می‌دهد.

</div>

<div>
[1]: 
<br>
[2]: 
<br>
[3]: 
<br>
[4]: 
<br>
[5]: 
<br>
[6]: 
<br>
[7]: 
<br>
[8]: 
<br>
[9]: 
<br>
[10]: 
<br>
[11]: 
<br>
[12]: 
<br>
[13]: 
<br>
[14]: 
<br>
[15]: 
<br>
[16]: 
<br>
[17]: 
<br>
[18]: 
<br>
[19]: 
<br>
[20]: 
<br>
[21]: 
<br>
[22]: 
<br>
[23]: 
<br>
[24]: 
<br>
[25]: 
<br>
[26]: 
<br>
[27]: 
<br>
[28]: 
</div>

