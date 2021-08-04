
<div dir='rtl'>

# فصل چهارم زیبا سازی

</div>
<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/4-Aesthetics/img-4-1.png" />
</p>

<div dir='rtl'>

در صفحه آرایی یک مجله موارد بسیاری دخیل است. طول پاراگراف‌ها، اندازه عرض ستون‌ها، ترتیب مقالات و اینکه چه چیزی روی جلد قرار بگیرد. طراحی خوب یک مجله باعث می‌شود که از صفحه‌ای به صفحه دیگر بروید و خواندن آن نیز روان باشد.

سورس کد خوب باید به آسانی به چشم بیاید. در این فصل نشان خواهیم داد که استفاده صحیح از فاصله گذاشتن1، تراز بندی2 و ترتیب3 بخش‌های مختلف چگونه می‌تواند خوانایی کد شما را آسان کند.

به طور خاص، در اینجا سه اصل داریم:

* همواره از طرح بندی(layout)‌ با الگوهایی که خواننده می‌تواند از آن‌ها استفاده کند، استفاده کنید.
* کدهای مشابه را از نظر ظاهری شبیه به هم بنویسید.
* خطوط مرتبطِ کد را در یک بلوک گروه‌بندی کنید.

## زیباسازی4 در مقایسه با طراحی

ما در این فصل فقط به دنبال ایجاد یک بهبود ساده در کد با استفاده از روش زیباسازی هستیم. ایجاد این نوع تغییرات، ساده بوده و اغلب خوانایی کد را کمی بهبود می‌دهد. البته زمان‌هایی نیز وجود دارد که یک بازسازی بزرگ می‌تواند کمک بیشتری به شما کند. به نظر ما زیباسازی خوب و طراحی خوب، ایده‌های مستقلی هستند که در حالت ایده‌آل باید برای هر دو تلاش کنید.

## چرا زیباسازی مهم است؟

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/4-Aesthetics/img-4-2.png" />
</p>

تصور کنید که مجبور باشید از کلاس زیر استفاده کنید:

</div>

```
class StatsKeeper {
    public:
    // A class for keeping track of a series of doubles
       void Add(double d);  // and methods for quick statistics about them
      private:   int count;        /* how many so    far
    */ public:
            double Average();
    private:   double minimum;
    list<double>
      past_items
          ;double maximum;
    };
    
```
<div dir='rtl'>

بی شک زمان مورد نیاز برای فهمیدن این کد نسبت به نسخه مرتب و تمیز شده زیر بسیار بیشتر خواهد بود:

</div>

```
// A class for keeping track of a series of doubles
// and methods for quick statistics about them.
class StatsKeeper {
    public:
      void Add(double d);
      double Average();
  
    private:
      list<double> past_items;
      int count;  // how many so far
      double minimum;
      double maximum;
  };    
```
<div dir='rtl'>

بدیهی است که کار با کدی که از نظر زیبایی ظاهری خوشایندتر باشد، ساده‌تر خواهد بود. اگر درباره آن فکر کنید، متوجه خواهید شد که بیشتر وقت برنامه‌نویسی شما صرف نگاه کردن به کد می‌شود! پس هرچه بتوانید به شکل سطحی کد خود را سریعتر بخوانید، استفاده از آن نیز برای دیگران آسانتر خواهد بود.

## تنظیم مجدد خطوط شکسته به صورت استوار1 و جمع و جور2

فرض کنید که شما در حال نوشتن کد Java برای ارزیابی نحوه رفتار برنامه خود، تحت سرعت‌های مختلفِ اتصال به شبکه هستید. شما یک TcpConnectionSimulator دارید که در سازنده3 خود چهار پارامتر دریافت می‌کند:

1. سرعت اتصال(Kbps)
2. میانگین تاخیر4(ms)
3. میزان Jitter 5تاخیر(ms)
4. میزان از دست دادن بسته‌ها6(درصد)

کد شما سه نمونه7 مختلف TcpConnectionSimulator نیاز دارد:

</div>

```
public class PerformanceTester {
    public static final TcpConnectionSimulator wifi = new TcpConnectionSimulator(
        500, /* Kbps */
        80, /* millisecs latency */
        200, /* jitter */
        1 /* packet loss % */);
    public static final TcpConnectionSimulator t3_fiber =
        new TcpConnectionSimulator(
            45000, /* Kbps */
            10, /* millisecs latency */
            0, /* jitter */
            0 /* packet loss % */);
    public static final TcpConnectionSimulator cell = new TcpConnectionSimulator(
        100, /* Kbps */
        400, /* millisecs latency */
        250, /* jitter */
        5 /* packet loss % */);
}    
```
<div dir='rtl'>

این مثال به تعداد زیادی خطوط کد اضافی محتاج است تا داخل یک محدودیت 80 کاراکتری (که استاندارد کدنویسی در شرکت است) قرار بگیرد. متاسفانه این امر باعث شده که تعریف t3_fiber نسبت به خطوط دیگر متفاوت به نظر رسیده و بدون هیچ دلیلی سبب جلب توجه کلمه t3_fiber می‌شود(اگر با دقت به کد نگاه کنید متوجه می‌شوید که عبارت new TcpConnectionSimulator در تعریف t3_fiber به خط بعدی رفته است).

این کد از اصلی که می‌گوید کدهای مشابه باید شبیه هم به نظر برسند، پیروی نمی‌کند. برای رعایت این اصل، می‌توانیم در هر نمونه بعد از علامت = یک Enter بزنیم تا کد جدید به شکل زیر شود:

</div>

```
public class PerformanceTester {
    public static final TcpConnectionSimulator wifi =
        new TcpConnectionSimulator(
            500,   /* Kbps */
            80,    /* millisecs latency */
            200,   /* jitter */
            1      /* packet loss % */);
    public static final TcpConnectionSimulator t3_fiber =
        new TcpConnectionSimulator(
            45000, /* Kbps */
            10,    /* millisecs latency */
            0,     /* jitter */
            0      /* packet loss % */);
    public static final TcpConnectionSimulator cell =
        new TcpConnectionSimulator(
            100,   /* Kbps */
            400,   /* millisecs latency */
            250,   /* jitter */
            5      /* packet loss % */);
}
    
```
<div dir='rtl'>

این کد از نظر الگو، زیبایی خوبی داشته و خوانایی آن در یک نگاه، ساده‌تر است، اما متاسفانه از فضای عمودی زیادی استفاده می‌کند. همچنین هر کامنت سه مرتبه تکرار شده است.

</div>

```
public class PerformanceTester {
    // TcpConnectionSimulator(throughput, latency, jitter, packet_loss)
    //                            [Kbps]   [ms]    [ms]    [percent]
    public static final TcpConnectionSimulator wifi =
        new TcpConnectionSimulator(500,    80,     200,    1);
    public static final TcpConnectionSimulator t3_fiber =
        new TcpConnectionSimulator(45000,  10,     0,      0);
    public static final TcpConnectionSimulator cell =
        new TcpConnectionSimulator(100,    400,    250,    5);
}
    
```
<div dir='rtl'>

ما کامنت‌ها را در قسمت بالا و همه پارامترها را در یک خط قرار داده‌ایم. حال، اگرچه این کامنت درست در جلوی شما نیست اما در عوض برنامه با خطوط جمع و جورتری به شکل ستون‌های یک جدول بصورت مرتب ارائه شده است.

## از متدها برای پاک کردن بی نظمی استفاده کنید

فرض کنید یک دیتابیس پرسنلی دارید که تابع زیر را ارائه می‌دهد:


</div>

```

// Turn a partial_name like "Doug Adams" into "Mr. Douglas Adams".
// If not possible, 'error' is filled with an explanation.
string ExpandFullName(DatabaseConnection dc, string partial_name, string* error);
    
```
<div dir='rtl'>

و این تابع توسط تعدادی از مثال‌ها تست می‌شود:
</div>

```

DatabaseConnection database_connection;
string error;
assert(ExpandFullName(database_connection, "Doug Adams", &error)
    == "Mr. Douglas Adams");
assert(error == "");
assert(ExpandFullName(database_connection, " Jake  Brown ", &error)
    == "Mr. Jacob Brown III");
assert(error == "");
assert(ExpandFullName(database_connection, "No Such Guy", &error) == "");
assert(error == "no match found");
assert(ExpandFullName(database_connection, "John", &error) == "");
assert(error == "more than one result");
    
```
<div dir='rtl'>

این کد از نظر زیباییِ ظاهری خوشایند نیست. برخی خطوط بسیار طولانی بوده و ادامه آن‌ها به خط بعدی رفته است و همچنین ظاهر کد زشت بوده و الگوی مرتبی در آن وجود ندارد. هرچند این مورد تنها با تنظیم مجدد خطوط شکسته قابل حل است ولی مشکل بزرگتر این است که تعداد زیادی از رشته‌ها شبیه «assert(ExpandFullName(database_connection» تکرار شده‌اند و عبارت «error» نیز به همین شکل است.

برای بهتر کردن این کد، نیازمند یک متد کمکی هستیم تا کد بتواند به شکل زیر نمایش داده شود:

</div>

```

CheckFullName("Doug Adams", "Mr. Douglas Adams", "");
CheckFullName(" Jake  Brown ", "Mr. Jake Brown III", "");
CheckFullName("No Such Guy", "", "no match found");
CheckFullName("John", "", "more than one result");
    
```
<div dir='rtl'>

اکنون این کد در نشان دادن اینکه چهار تست آن هم با پارامترهای متفاوت در حال رخ دادن است، شفاف‌تر به نظر می‌رسد. حتی اگر **کارهای کثیف** 1 داخل تابع CheckFullName() باشد، این تابع چندان هم بد نخواهد بود:

</div>

```

 void CheckFullName(string partial_name,
    string expected_full_name,
    string expected_error) {
// database_connection is now a class member
string error;
string full_name = ExpandFullName(database_connection, partial_name, &error);
assert(error == expected_error);
assert(full_name == expected_full_name);
}
    
```
<div dir='rtl'>

اگر هدف ما فقط این باشد که کد را از نظر زیباسازی خوشایندتر کنیم، این تغییر مزایای جانبی دیگری نیز دارد:

* سبب از بین رفتن کدهای تکراری زیادی نسبت به قبل شده که نتیجه آن جمع و جورتر شدن کد خواهد بود.
* مهمترین بخش از هر مورد تست1 یعنی نام‌ها و رشته‌های خطا2 اکنون به طور واضح و در یک سمت مشخص قرار گرفته است.
* قبلا این رشته‌ها با علامت‌هایی3 مانند database_connection و error پراکنده بودند که سبب سخت شدن درک آن‌ها با یک نگاه می‌شد.
* اکنون باید افزودن تست‌های جدید ساده‌تر باشد.
* نکته اخلاقی داستان این است که زیبا کردن یک کد، اغلب به چیزی بیشتر از  یک بهبود ظاهری، منجر می‌شود و این کار ممکن است به بهتر شدن ساختار کد شما نیز کمک کند.

## در صورت مفید بودن از ترازبندی ستونی4 استفاده کنید

در یک نگاه گوشه‌ها و ستون‌های صاف خوانایی راحت‌تری را برای خواننده به همراه دارد. گاهی اوقات می‌توانید «ترازبندی ستون» را برای ساده‌تر کردن خواندن کد، معرفی کنید. به عنوان مثال در بخش قبلی شما می‌توانستید از فضای خالی بعد از کلمات در آرگومان‌های تابع CheckFullName() استفاده کنید:

</div>

```

CheckFullName("Doug Adams"   , "Mr. Douglas Adams" , "");
CheckFullName(" Jake  Brown ", "Mr. Jake Brown III", "");
CheckFullName("No Such Guy"  , ""                  , "no match found");
CheckFullName("John"         , ""                  , "more than one result");
    
```
<div dir='rtl'>

در این کد، تمایز قائل شدن بین آرگومان‌های دوم و سوم تابع CheckFullName() ساده‌تر است. 

در اینجا مثالی ساده با یک گروه بندی بزرگ از تعریف متغیرها را بررسی می‌کنیم:

</div>

```

# Extract POST parameters to local variables
details  = request.POST.get('details')
location = request.POST.get('location')
phone    = equest.POST.get('phone')
email    = request.POST.get('email')
url      = request.POST.get('url')
    
```
<div dir='rtl'>

همان گونه که احتمالا متوجه شده‌اید، تعریف سوم یک اشتباه تایپی دارد(به جای کلمه request کلمه equest نوشته شده است). اشتباهات این چنینی وقتی همه چیز به صورت مرتب نوشته شده باشد، به شکل برجسته‌تری ظاهر می‌شوند.

در کد پایه دستور wget، گزینه‌های موجود در خط فرمان(بیشتر از ۱۰۰ مورد از آن‌ها) به شکل زیر لیست شده‌اند:

</div>

```

commands[] = {
    ...
    { "timeout",          NULL,                   cmd_spec_timeout },
    { "timestamping",     &opt.timestamping,      cmd_boolean },
    { "tries",            &opt.ntry,              cmd_number_inf },
    { "useproxy",         &opt.use_proxy,         cmd_boolean },
    { "useragent",        NULL,                   cmd_spec_useragent },
    ...
};
    
```
<div dir='rtl'>

این شیوه باعث شده است که این لیست به راحتی و در یک نگاه قابل خواندن بوده و پریدن از یک ستون به ستون بعدی خیلی ساده باشد.

## آیا باید از ترازبندی ستونی استفاده کرد؟

لبه‌های ستون مانند نرده‌های راه پله هستند که دنبال کردن کد را آسانتر می‌کنند.(مانند زمانی که از پله‌ها بالا می‌روید و نرده‌ها را با دست خود می‌گیرید). این یک مثال خوب از «کدهای مشابه را شبیه هم بسازید» است. 

اما بعضی از برنامه‌نویسان این شیوه را نمی‌پسندند. یک دلیل این است که این شیوه به کار بیشتری جهت راه اندازی و حفظ ترازبندی نیاز دارد. دلیل دیگر این است که این شیوه هنگام ایجاد تغییرات «تفاوت1» بزرگی را ایجاد می‌کند چرا که اگر یک خط تغییر کند احتمالا سبب تغییر پنج خط دیگر می‌شود که این تغییر در اکثر موارد به دلیل وجود فضای خالی است.

پیشنهاد ما این است که این شیوه را امتحان کنید. در تجربه ما، آن قدر هم که برنامه‌نویسان می‌ترسند هم طول نمی‌کشد و اگر احیاناً این کار انرژی و وقت زیادی از شما گرفت به راحتی می‌توانید آن را ادامه ندهید.

## یک ترتیب معنا دار2 انتخاب و از آن به طور مداوم استفاده کنید

موارد زیادی وجود دارد که ترتیب کد روی درستی3 آن اثر نمی‌گذارد، به عنوان مثال، تعریف پنج متغیر زیر به هر ترتیبی می‌تواند نوشته شود:

</div>

```

details  = request.POST.get('details')
location = request.POST.get('location')
phone    = request.POST.get('phone')
email    = request.POST.get('email')
url      = request.POST.get('url')    

```
<div dir='rtl'>

در شرایط مشابه، اینکه آن‌ها  را به یک ترتیب معنادار و نه صرفا به شکل تصادفی بنویسید، مفید خواهد بود. چند ایده برای این کار وجود دارد:

* ترتیب متغیرها را با ترتیب فیلدهای <input> مربوط به فرم HTML مطابقت دهید.
* آن‌ها را از «اهمیت بیشتر» به  «اهمیت کمتر» مرتب کنید.
* آن‌ها را به صورت «ترتیب حروف الفبا» مرتب کنید.

ولی به یاد داشته باشید که ترتیب به هر صورتی که باشد، باید از همان ترتیب در سراسر کد خود استفاده کنید، زیرا تغییر ترتیب در بخش‌های دیگر کد، می‌تواند باعث سردرگمی خواننده شود:
</div>

```

if details:  rec.details  = details
if phone:    rec.phone    = phone     # Hey, where did 'location' go?
if email:    rec.email    = email
if url:      rec.url      = url
if location: rec.location = location  # Why is 'location' down here now?
    
```
<div dir='rtl'>

## اعلان‌ها 1 را در یک بلوک قرار دهید

از آنجا که مغز انسان به طور طبیعی به ساختار و قواعد گروه بندی‌ها و سلسله مراتب آن‌ها دقت می‌کند، می‌توانید با ساماندهی ساختار به خواننده کمک کنید تا کدهای شما را سریع‌تر و به شکل خلاصه درک کند. به عنوان مثال در اینجا یک کلاس C++ برای فرانت‌اند یک سرور با تمام اعلان‌های متد2 داریم:

</div>

```

class FrontendServer {
    public:
      FrontendServer();
      void ViewProfile(HttpRequest* request);
      void OpenDatabase(string location, string user);
      void SaveProfile(HttpRequest* request);
      string ExtractQueryParam(HttpRequest* request, string param);
      void ReplyOK(HttpRequest* request, string html);
      void FindFriends(HttpRequest* request);
      void ReplyNotFound(HttpRequest* request, string error);
      void CloseDatabase(string location);
      ~FrontendServer();
  };
    
```
<div dir='rtl'>

هرچند این کد وحشتناک نیست، اما مطمئنا طرح‌بندی آن کمکی به خواننده برای درک همه متدها با یک نگاه نمی‌کند. به جای قرار دادن متدها در یک بلوک غول پیکر، باید از نظر منطقی این گونه گروه‌بندی شوند:

</div>

```

class FrontendServer {
    public:
      FrontendServer();
      ~FrontendServer();
      // Handlers
      void ViewProfile(HttpRequest* request);
      void SaveProfile(HttpRequest* request);
      void FindFriends(HttpRequest* request);
      // Request/Reply Utilities
      string ExtractQueryParam(HttpRequest* request, string param);
      void ReplyOK(HttpRequest* request, string html);
      void ReplyNotFound(HttpRequest* request, string error);
          // Database Helpers
    void OpenDatabase(string location, string user);
    void CloseDatabase(string location);
};
    
```
<div dir='rtl'>

این نسخه از کد خیلی راحت‌تر و در یک نگاه درک شده و برای خواندن نیز ساده‌تر است، حتی اگر خطوط کد بیشتری وجود داشته باشد، چرا که شما سریعا متوجه وجود چهار بخش با اهمیت بالاتر شده و سپس هر زمان که لازم شد جزئیات هر بخش را می‌خوانید.

##تقسیم‌بندی کد به صورت پاراگراف‌ها1

یک متن نوشتاری به چندین دلیل به پاراگراف‌های جداگانه تقسیم می‌شود:

* این کار راهی برای گروه‌بندی ایده‌های مشابه با یکدیگر و جدا کردن شان از دیگر ایده‌ها است.
* این کار یک رد پای2 بصری فراهم می‌کند که بدون آن، به راحتی ممکن است مکان خود را در صفحه گم کنید.
* این کار رفتن از یک پاراگراف به پاراگراف دیگر را تسهیل می‌کند.

یک کد برنامه نویسی شده نیز به دلایل مشابه باید به پاراگراف‌هایی تقسیم شود. به عنوان مثال، کسی علاقه‌مند به خواندن یک کد عظیم مانند این نیست:

</div>

```

# Import the user's email contacts, and match them to users in our system.
# Then display a list of those users that he/she isn't already friends with.
def suggest_new_friends(user, email_password):
    friends = user.friends()
    friend_emails = set(f.email for f in friends)
    contacts = import_contacts(user.email, email_password)
    contact_emails = set(c.email for c in contacts)
    non_friend_emails = contact_emails - friend_emails
    suggested_friends = User.objects.select(email__in=non_friend_emails)
    display['user'] = user
    display['friends'] = friends
    display['suggested_friends'] = suggested_friends
    return render("suggested_friends.html", display)

```

<div dir='rtl'>

هرچند این کد واضح نیست، اما از آنجا که این تابع چندین مرحله متمایز را طی می‌کند، تفکیک خطوط آن توسط پاراگراف‌ها، برای درک آن بسیار مفید خواهد بود:
</div>

```

def suggest_new_friends(user, email_password):
    # Get the user's friends' email addresses.
    friends = user.friends()
    friend_emails = set(f.email for f in friends)
    # Import all email addresses from this user's email account.
    contacts = import_contacts(user.email, email_password)
    contact_emails = set(c.email for c in contacts)
    # Find matching users that they aren't already friends with.
    non_friend_emails = contact_emails - friend_emails
    suggested_friends = User.objects.select(email__in=non_friend_emails)
    # Display these lists on the page.
    display['user'] = user
    display['friends'] = friends
    display['suggested_friends'] = suggested_friends
    return render("suggested_friends.html", display)
    
```

<div dir='rtl'>

نکته‌ای که ما آن را در بخش خلاصه فصل نیز بیان خواهیم کرد این است که هر پاراگراف را کامنت گذاری کنید، این کار سبب می‌شود خواننده به راحتی و در یک نگاه کد را درک کند(فصل ۵ «دانستن اینکه چه چیزی را کامنت کنیم» را مشاهده کنید).

همچون یک متن نوشتاری، ممکن است چندین روش برای تقسیم‌بندی کدها وجود داشته باشد و هر برنامه‌نویسی ممکن است پاراگراف‌های کوچک‌تر یا بزرگتر را ترجیح دهد.

## سبک شخصی یا قوانین و قواعد!

گزینه‌های زیباسازی خاصی وجود دارد که فقط در استایل شخصی مهم‌اند. برای نمونه جایی که براکت برای تعریف یک کلاس باز شده است، به این شکل:
</div>

```

experiment_id: 101
reuse: 100
..

```

<div dir='rtl'>

کلمه reuse مناسب است اما همان گونه که قبلا گفتیم ممکن است کسی برداشتش این باشد که این آزمایش حداکثر ۱۰۰ مرتبه قابل استفاده مجدد است. تغییر این نام به reuse_id می‌تواند کمک کننده باشد. اما ممکن است دوباره خواننده گیج شده و فکر کند که عبارت reuse_id: 100 به این معنی است که شناسه من برای استفاده مجدد 100 است.

3. اکنون copy‌ را در نظر بگیرید:

</div>

```

class Logger {
    ...
};

```
<div dir='rtl'>

و یا به این شکل باشد:

</div>

```

class Logger
{
    ...
};	

```

<div dir='rtl'>

اگرچه ترجیح دادن یکی از این استایل‌ها به میزان قابل توجهی روی خوانایی کدپایه1 اثر نمی‌گذارد، ولی اگر این دو استایل هم زمان و به شکل مخلوط در کل کد استفاده شوند، روی خوانایی کد اثر خواهد گذاشت. 

هر چند ما پروژه‌های زیادی را با این احساس که شبیه تیمی هستیم که از استایل اشتباه استفاده می‌کند، انجام داده‌ایم، ولی در عین حال قواعد2 پروژه را دنبال می‌کردیم، چرا که می‌دانستیم قواعد تیم بسیار مهم‌تر هستند.

> **_کلید طلایی:_**  استایل‌های همیشگی از استایل «مناسب1» بهتر هستند.

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/4-Aesthetics/img-4-3.png" />
</p>

## خلاصه فصل

هر کسی ترجیح می‌دهد کدهایی را که از نظر زیبایی ظاهری خوشایند هستند بخواند. با قالب بندی1 کدهای خود به صورت مداوم، به روشی معنادار، خواندن آن را ساده‌تر و سریعتر می‌کنید.

در اینجا تکنیک‌های خاصی که در طول این فصل به آن‌ها پرداختیم آورده شده است:

* اگر چندین بلوک از کد، کاری مشابه را انجام می‌دهند، سعی کنید آن‌ها را از حیث زیباسازی به یک شکل بنویسید.
* ترازبندی2 کد در ستون‌ها3 می‌تواند نگاه گذرا به کد را ساده‌تر کند. اگر کد در جایی به A، B و C اشاره می‌کند، در مکان دیگر آن را به صورت B، C و A ننویسید. ترتیبی مشخص و معنی‌دار انتخاب کرده و به آن پایبند باشید.

از خطوط خالی برای تقسیم‌بندی بلوک‌های بزرگ به پاراگراف‌های منطقی استفاده کنید.

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

