
<div dir='rtl'>

# فصل دوازدهم تبدیل افکار به کد

</div>

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/12-Turning-Thoughts-into-Code/img-12-1.png" />
</p>

<div dir='rtl'>

<p align="center">

**زمانی شما واقعا چیزی را فهمیده‌اید که بتوانید آن را به مادربزرگ خود توضیح دهید.**

**(آلبرت انیشتین)**
</p>

هنگام توضیح یک ایده پیچیده برای یک شخص، بیان همه جزئیات به راحتی او را دچار سردرگمی می‌کند.تسلط بر توضیح یک ایده به زبان ساده به گونه‌ای که شخصی که از شما دانش کمتری دارد، بتواند آن را درک کند، مهارتی ارزشمند است. لازمه این کار، توان خلاصه کردن یک ایده در مهم‌ترین مفاهیم آن است. انجام این کار علاوه بر کمک به درک دیگران برای شما نیز این فایده را دارد که می‌توانید در مورد ایده خود با وضوح بیشتری فکر کنید.

در هنگام ارائه کد به خواننده باید از این مهارت استفاده کنید. معتقدیم مهم‌ترین وسیله برای توضیح نحوه کارکرد برنامه شما، سورس کدتان است، بنابراین باید کد را به زبان انگلیسی ساده بنویسید.

در این فصل از فرآیندی ساده استفاده خواهیم کرد که می‌تواند به شما در نوشتن یک کد واضح‌تر  کمک کند:

1. به عنوان یک همکار آنچه را کد برای انجام نیاز دارد به زبان ساده، توضیح دهید.
2. به کلمات کلیدی و عبارت‌های استفاده شده در این توضیحات توجه کنید.
3. کد خود را مطابق با این توضیحات بنویسد.

## توضیح منطق کد به طور شفاف

در اینجا قطعه‌ای از یک کد به زبان PHP از یک صفحه وب داریم. این کد در بالای یک صفحه امن قرار دارد که بررسی می‌کند آیا کاربر مجاز1 به دیدن این صفحه است یا نه؟ اگر مجاز نباشد، بلافاصله صفحه‌ای را نشان می‌دهد که به کاربر می‌گوید اجازه دیدن صفحه را ندارد:

</div>

```

$is_admin = is_admin_request();
if ($document) {
    if (!$is_admin && ($document['username'] != $_SESSION['username'])) {
        return not_authorized();
    }
} else {
    if (!$is_admin) {
        return not_authorized();
    }
}
// continue rendering the page ...

```
<div dir='rtl'>

منطق این کد هنوز به طور کامل، تکمیل نشده است. همان گونه که از بخش دوم(ساده سازی حلقه‌ها و منطق) به یاد دارید، درک چنین منطق‌های تودرتویی ساده نیست. احتمالا منطق این کد می‌تواند ساده‌تر شود، اما چگونه؟ اجازه دهید با توصیف منطق به زبان ساده شروع کنیم:

**دو روش برای مجاز بودن شما جهت دیدن صفحه وب وجود دارد:**

1. شما admin هستید.
2. شما مالک سند فعلی هستید(اگر سندی وجود داشته باشد)
**در غیر این صورت، شما مجاز نیستید.**

روشی جایگزین با الهام از این توضیحات این است:

</div>

```

if (is_admin_request()) {
    // authorized
} elseif ($document && ($document['username'] == $_SESSION['username'])) {
    // authorized
} else {
    return not_authorized();
}
// continue rendering the page ...

```
<div dir='rtl'>

هر چند این نسخه به دلیل وجود دو قسمت خالی کمی غیرمعمول است، اما دارای کدی کوتاه‌تر و منطقی ساده‌تر است، زیرا هیچ نفی1 کردنی وجود ندارد(در حالی که راه حل قبلی دارای سه «not» بود). همچنین درک آن نیز آسان‌تر می‌باشد.

## اطلاع از کتابخانه‌های‌تان می‌تواند مفید باشد

زمانی ما یک وبسایتی داشتیم که شامل یک «جعبه نکات1» بود و پیشنهادات مفیدی را به کاربر نشان می‌داد. مانند:

</div>

```

Tip: Log in to see your past queries. [Show me another tip!]

```
<div dir='rtl'>

در آن جا هزاران نکته وجود داشت که همگی در داخل کد HTML مخفی شده بود:

</div>

```

<div id="tip-1" class="tip">Tip: Log in to see your past queries.</div>
<div id="tip-2" class="tip">Tip: Click on a picture to see it close up.</div>
...

```
<div dir='rtl'>

هنگامی بازدید یک کاربر از صفحه، یکی از این div‌ها به صورت تصادفی قابل نمایش(visible) شده و بقیه divها در حالت مخفی(hidden) باقی می‌مانند و اگر روی لینک «Show me another tip!» کلیک شود، نکته بعدی نمایش داده خواهد داد. در اینجا برخی از کدهایی که برای پیاده سازی این ویژگی با استفاده از کتابخانه jQuery نوشته شده بود، آورده شده است:

</div>

```

var show_next_tip = function () {
    var num_tips = $('.tip').size();
    var shown_tip = $('.tip:visible');
    var shown_tip_num = Number(shown_tip.attr('id').slice(4));
    if (shown_tip_num === num_tips) {
        $('#tip-1').show();
    } else {
        $('#tip-' + (shown_tip_num + 1)).show();
    }
    shown_tip.hide();
};

```
<div dir='rtl'>

هرچند این کد خوبی است اما می‌تواند بهتر از این شود. اجازه دهید ابتدا در جملاتی توصیف کنیم که این کد(هنگام کلیک کاربر) تلاش می‌کند چه کاری انجام دهد:

**نکته فعلی قابل مشاهده را پیدا کرده و آن را مخفی کنید.**
**سپس نکته بعدی را پیدا کرده و آن را نمایش دهید.**
**اگر نکته بعدی وجود نداشت به ابتدای چرخه بر می‌گردیم.**

راه حل جدید، مبتنی بر این توضیحات به این صورت است:

</div>

```

var show_next_tip = function () {
    var cur_tip = $('.tip:visible').hide();  // find the currently visible tip and hide it               
    var next_tip = cur_tip.next('.tip');     // find the next tip after it
    if (next_tip.size() === 0) {             // if we've run out of tips,
        next_tip = $('.tip:first');          //     cycle back to the first tip
    }
    next_tip.show();                         // show the new tip
};

```
<div dir='rtl'>

این راه حل علاوه بر داشتن خطوط کمتر، نیازی به دستکاری مستقیم مقادیر integer ندارد. 

همچنین تطابق بیشتری با نحوه تفکر یک شخص در مورد آن دارد.در این راهکار استفاده از متدی به نام next()، که متعلق jQuery است، کمک کننده است. به یاد داشته باشید که یکی از ابزارهای مختصر نویسی کد این است که، از آنچه که کتابخانه شما عرضه کرده است، آگاه باشید.

## اعمال این رویکرد در مسئله‌های بزرگتر

مثال‌های قبلی فرآیند ما را در بلوک‌های کوچکی از کد اعمال کرده اند. در مثال بعدی، ما آن‌ها را در یک تابع بزرگتر اعمال می‌کنیم. همان گونه که خواهید دید، این متد می‌تواند با شناسایی بخشی از کد، که می‌تواند شکسته شود، به شما کمک کند. تصور کنید که سیستمی برای ثبت خرید سهام داریم. هر تراکنش چهار قسمت از داده را دارد:


* time (یک تاریخ دقیق و زمان خرید)
* ticker_symbol (e.g., GOOG)
* price (e.g., $600)
* number_of_shares (e.g., 100)

به دلایل عجیبی، داده‌ها در سه جدول جداگانه دیتابیس، بصورت زیر پخش شده و در هر یک از آن‌ها، time کلید اصلی1 منحصر به فرد2 است.

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/12-Turning-Thoughts-into-Code/img-12-2.png" />
</p>

حال ما باید برنامه‌ای برای پیوستن1 سه جدول به یکدیگر بنویسیم(همان گونه که یک عملیات JOIN در SQL انجام می‌شود). این مرحله باید ساده باشد زیرا سطرها همه بر اساس time مرتب شده‌اند اما متاسفانه برخی از سطر‌ها مفقود شده‌اند. شما می‌خواهید همه سطر‌هایی که time‌های هر سه آن‌ها با یکدیگر مطابقت دارد را پیدا کنید، و هر سطری را که نمی‌توان مرتب کرد را نادیده2 بگیرید.

در اینجا کدی به زبان Python داریم که همه سطرهای منطبق را پیدا می‌کند:

</div>

```

def PrintStockTransactions():
    stock_iter = db_read("SELECT time, ticker_symbol FROM ...")
    price_iter = ...
    num_shares_iter = ...
    # Iterate through all the rows of the 3 tables in parallel.
    while stock_iter and price_iter and num_shares_iter:
        stock_time = stock_iter.time
        price_time = price_iter.time
        num_shares_time = num_shares_iter.time
        # If all 3 rows don't have the same time, skip over the oldest row
        # Note: the "<=" below can't just be "<" in case there are 2 tied-oldest.
        if stock_time != price_time or stock_time != num_shares_time:
            if stock_time <= price_time and stock_time <= num_shares_time:
                stock_iter.NextRow()
            elif price_time <= stock_time and price_time <= num_shares_time:
                price_iter.NextRow()
            elif num_shares_time <= stock_time and num_shares_time <= price_time:
                num_shares_iter.NextRow()
            else:
                assert False  # impossible
            continue
        assert stock_time == price_time == num_shares_time
       
        # Print the aligned rows.
        print "@", stock_time,
        print stock_iter.ticker_symbol,
        print price_iter.price,
        print num_shares_iter.number_of_shares
        stock_iter.NextRow()
        price_iter.NextRow()
        num_shares_iter.NextRow()

```
<div dir='rtl'>

هر چند این کد کار می‌کند، اما چیزهای زیادی در مورد چگونگی گذر حلقه از سطرهای غیرمنطبق1 وجود دارد. ممکن است برخی از علائم هشدار در ذهن شما خاموش شده باشد: آیا این کد می‌تواند برخی از سطرها را از دست بدهد؟ آیا ممکن است که مقدار قبلی، پایان یک جریان را در هر تکرار بخواند؟ چگونه می‌توانیم این کد را خواناتر کنیم؟

**یک توضیح به زبان ساده از راه حل**

بار دیگر، بیایید به عقب بازگشته و به زبان ساده توضیح دهیم که در صدد انجام چه کاری هستیم:

ما در حال خواندن سه سطر تکرار شونده به شکل موازی هستیم.

**هر زمان که time سطرها منطبق نیستند، سطرها را پیش ببرید تا آن‌ها منطبق شوند.**

سپس سطرهای هم‌تراز شده را چاپ کرده و دوباره سطرها را پیش ببرید.

این کار را تا زمانی که هیچ سطر منطبقی باقی نماند، ادامه دهید.

به عقب برگردید و به کد اصلی نگاه کنید. آشفته‌ترین قسمت، بلوک تعامل با «قسمت پیش‌روی سطرها تا آن‌ها با هم منطبق شوند» بود. برای نمایش کد با شفافیت بیشتر، می‌توانیم منطق آشفته را از کد استخراج کرده و در تابع جدیدی به نام AdvanceToMatchingTime() قرار دهیم.

در اینجا نسخه جدیدی از کد وجود دارد که از این تابع جدید استفاده می‌کند:

</div>

```

def PrintStockTransactions():
    stock_iter = ...
    price_iter = ...
    num_shares_iter = ...
    while True:
        time = AdvanceToMatchingTime(stock_iter, price_iter, num_shares_iter)
        if time is None:
            return
        # Print the aligned rows.
        print "@", time,
        print stock_iter.ticker_symbol,
        print price_iter.price,
        print num_shares_iter.number_of_shares
        stock_iter.NextRow()
        price_iter.NextRow()
        num_shares_iter.NextRow()

```
<div dir='rtl'>

همان گونه که مشاهده می‌کنید، درک این کد بسیار ساده‌تر است، زیرا ما همه جزئیات مبهم، در مورد تطابق سطرها را مخفی کردیم.

## اعمال متد به صورت بازگشتی1

تصور اینکه چگونه AdvanceToMatchingTime() را می‌نویسید ساده است. در بدترین حالت، بسیار شبیه به بلوک کد زشتی است که در نسخه اول موجود است:

</div>

```

def AdvanceToMatchingTime(stock_iter, price_iter, num_shares_iter):
    # Iterate through all the rows of the 3 tables in parallel.
    while stock_iter and price_iter and num_shares_iter:
        stock_time = stock_iter.time
        price_time = price_iter.time
        num_shares_time = num_shares_iter.time
        # If all 3 rows don't have the same time, skip over the oldest row
        if stock_time != price_time or stock_time != num_shares_time:
            if stock_time <= price_time and stock_time <= num_shares_time:
                stock_iter.NextRow()
            elif price_time <= stock_time and price_time <= num_shares_time:
                price_iter.NextRow()
            elif num_shares_time <= stock_time and num_shares_time <= price_time:
                num_shares_iter.NextRow()
            else:
                assert False  # impossible
            continue
            assert stock_time == price_time == num_shares_time
            return stock_time

```
<div dir='rtl'>

حال بیایید این کد را با اعمال شیوه خودمان در AdvanceToMatchingTime() بهبود دهیم. در اینجا توضیحی در مورد آنچه که این متد باید انجام دهد را داریم:

**به زمان هر سطر فعلی نگاه کنید: اگر آن‌ها تراز1 شده‌اند، کار ما تمام است.**
**در غیر این صورت، به سمت سطرهای قبلی بروید.**
**این کار را تا زمانی که همه سطرها تراز شده باشند(یا یکی از این تکرارها به پایان برسد) ادامه دهید.**

این توضیحات بسیار واضح، زیبا و موزون‌تر از کد قبلی است. نکته قابل توجه این است که این توضیحات هرگز اشاره‌ای به stock_iter یا دیگر جزئیات خاص در مورد مشکل ما نمی‌کند. این بدان معنی است که ما می‌توانیم متغیرها را نیز به شکل ساده‌تر و عمومی‌تری تغییر نام دهیم. در اینجا نتیجه این کد را می‌بینیم:

</div>

```

def AdvanceToMatchingTime(row_iter1, row_iter2, row_iter3):
    while row_iter1 and row_iter2 and row_iter3:   
        t1 = row_iter1.time
        t2 = row_iter2.time
        t3 = row_iter3.time
        if t1 == t2 == t3:
            return t1
        tmax = max(t1, t2, t3)
        # If any row is "behind," advance it.
        # Eventually, this while loop will align them all.
        if t1 < tmax: row_iter1.NextRow()
        if t2 < tmax: row_iter2.NextRow()
        if t3 < tmax: row_iter3.NextRow()
    return None  # no alignment could be found

```
<div dir='rtl'>

این کد خیلی شفاف‌تر از کد قبلی است. الگوریتم آن ساده‌تر شده و شرط‌های پیچیده کمتری دارد. ما از نام‌های کوتاهی مانند t1 استفاده کردیم که دیگر نیازی به فکر کردن در مورد ستون‌های1 خاص دیتابیس ندارد.

## خلاصه فصل

در این فصل در مورد تکنیک ساده توصیف برنامه به زبان ساده و استفاده از این توصیف‌ها برای کمک به نوشتن کد ساده‌تر بحث کردیم. این تکنیک به شکل فریبنده‌ای ساده، اما بسیار مفید است. نگاه کردن به کلمات و عبارت‌های استفاده شده در توصیفات می‌تواند در تشخیص اینکه کدام زیرمسئله‌ها باید شکسته شوند، به شما کمک کند. 

فرآیند «بیان کردن موارد به زبان ساده» کاربردهای دیگری نیز غیر از کمک در نوشتن کد دارد، به عنوان مثال یکی از همکاران ما در آزمایشگاه کامپیوتر بیان می‌کرد که، هر گاه یک دانشجو برای اشکال‌زدایی برنامه‌اش به کمک نیاز داشت، ابتدا باید مشکل را به یک خرس عروسکی که در گوشه اتاق بود، توضیح می‌داد. با کمال تعجب در اکثر موارد توصیف مسئله با صدای بلند، به دانشجو کمک می‌کرد تا متوجه راه حل شود. این تکنیک به اسم اردک پلاستیکی2 معروف است.

به یاد داشته باشید که اگر نمی‌توانید مشکل یا طراحی خود را با کلمات توصیف کنید، احتمالا چیزی را در نظر نگرفته‌اید یا در برنامه تعریف نشده است. بیان یک برنامه یا یک ایده با کلمات، می‌تواند آن را مجبور کند که به خودش شکل بگیرد.

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

