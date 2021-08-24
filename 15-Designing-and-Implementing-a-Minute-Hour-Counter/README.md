
<div dir='rtl'>

# فصل پانزدهم طراحی و پیاده سازی یک «شمارنده دقیقه/ساعت»

</div>

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/15-Designing-and-Implementing-a-Minute-Hour-Counter/img-15-1.png" />
</p>

<div dir='rtl'>

بیایید به ساختار داده استفاده شده در یک کد محصول واقعی نگاهی بیندازیم: یک «شمارنده دقیقه/ساعت». ما شما را با فرآیند طبیعی تفکر یک مهندس، آشنا می‌کنیم، ابتدا سعی در حل این مسئله داشته و سپس به بهبود کارایی و افزودن ویژگی‌هایی به آن خواهیم پرداخت، از همه مهم‌تر، سعی خواهیم کرد با استفاده از اصولی که در این کتاب بیان شده است، کد را برای خواندن ساده نگه داریم. البته ممکن است در این مسیر دچار چند اشتباه شویم. ببینید آیا می‌توانید آن‌ها را دنبال  کرده و تشخیص دهید؟

## مسئله

ما می‌خواهیم تعداد byte‌هایی که در یک دقیقه و همچنین یک ساعت گذشته در یک سرور وب منتقل شده است را ردیابی کنیم. در اینجا تصویری از چگونگی نگه‌داری آن‌ها داریم:

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/15-Designing-and-Implementing-a-Minute-Hour-Counter/img-15-2.png" />
</p>

در ابتدا به نظر می‌رسد که این یک مسئله نسبتا راحت است، اما همان گونه که خواهید دید، حل کردن آن به صورت کارآمد یک چالش جالب خواهد بود. بیایید با تعریف رابط کلاس شروع کنیم.

## تعریف Interface کلاس

اولین نسخه از Interface کلاس ما به زبان C++ به صورت زیر است:
</div>

```

class MinuteHourCounter {
    public:
      // Add a count
      void Count(int num_bytes);
      // Return the count over this minute
      int MinuteCount();
      // Return the count over this hour
      int HourCount();
  };

```
<div dir='rtl'>

قبل از پیاده سازی این کلاس، اجازه دهید از طریق مشاهده نام‌ها و کامنت‌ها، ببینیم چیزی وجود دارد که بخواهیم آن را تغییر دهیم یا نه؟

## بهبود نام‌ها

نام کلاس MinuteHourCounter خیلی خوب است. این نام خیلی خاص، مشخص و هنگام تلفظ آسان است. با توجه به نام کلاس، نام متدهای MinuteCount() و HourCount() نیز منطقی هستند. ممکن است شما آن‌ها را GetMinuteCount() و GetHourCount() بنامید اما این کار هیچ کمکی نمی‌کند. همان گونه که در فصل ۳ بیان کردیم، نام‌ها نباید گمراه کننده یا موجب برداشت‌های متناقض باشند، در حالی که کلمه get برای خیلی از افراد اشاره به «دسترسی سریع و سبک1» دارد و همان گونه که خواهید دید، پیاده سازی این مسئله، سبک نخواهد بود، بنابراین بهتر است از get در نام‌گذاری استفاده نکنیم.

ظاهرا نام متد Count() گیج کننده است. ما از همکاران خود سوال کردیم: به نظرتان Count() چه کاری انجام می‌دهد؟ عده‌ای تصور می‌کردند که این یعنی تعداد کل شمارش‌ها در کل زمان را برمی‌گرداند. این اسم کمی متناقض2 بوده و بدون هدف در نظر گرفته شده است. مشکل این است که (در زبان انگلیسی) Count هم اسم و هم فعل است و حتی می‌تواند به این معنی باشد که «من یک شمارش از تعداد نمونه‌هایی که تا کنون دیده‌اید را می‌خواهم» یا «من از شما می‌خواهم که این نمونه را بشمارید».

در اینجا نام‌های متناظر برای جایگزین شدن با Count() آورده شده است:
</div>

* Increment()
* Observe()
* Record()
* Add()

<div dir='rtl'>

Increment() هم گمراه کننده است زیرا به این معنی است که مقداری وجود دارد که فقط افزایش می‌یابد.(در مثال ما، شمارش ساعت به صورت چرخشی است). 

Observe() خوب است اما کمی مبهم است.

Record() نیز مشکل کلمه/فعل بودن را دارد، بنابراین گزینه خوبی نیست.

Add() جالب است زیرا می‌تواند به معنای «این را به شکل عددی اضافه کن» یا «اضافه کردن به یک لیست از داده» باشد. در مسئله ما، مقداری از مفهوم هر دو جمله وجود دارد، در نتیجه می‌تواند به کار آید. 

بنابراین ما متد خود را به void Add(int num_bytes) تغییر نام می‌دهیم. اما نام آرگومان num_bytes خیلی خاص است. بله، مورد کاربرد اصلی ما برای شمارش byteها است، اما نیازی نیست که MinuteHourCounter آن را بداند. ممکن است شخص دیگری از این کلاس برای شمارش کوئری‌ها یا تعاملات پایگاه داده استفاده کند. ما می‌توانیم از نام عمومی‌تری مانند delta استفاده کنیم، اما عبارت delta اغلب برای کاربرد‌هایی استفاده می‌شود که مقدار آن می‌تواند منفی باشد که ما چنین چیزی را نمی‌خواهیم. نام count باید بهتر باشد. این نام ساده و عمومی است و اشاره بر غیرمنفی بودن دارد. همچنین به ما اجازه می‌دهد که کاربر برداشت دوپهلوی کمتری از کلمه count داشته باشد.

## بهبود کامنت‌ها

تا اینجا این رابط کلاس را داریم:

</div>

```

class MinuteHourCounter {
    public:
      // Add a count
      void Add(int count);
      // Return the count over this minute
      int MinuteCount();
      // Return the count over this hour
      int HourCount();
  };

```
<div dir='rtl'>

بیایید هر یک از این متدها را کامنت گذاری کنیم و آن‌ها  را بهبود دهیم. اولین مورد را در نظر بگیرید:

</div>

```

// Add a count
void Add(int count);

```
<div dir='rtl'>

این کامنت به این شکل کاملا زائد است. یا باید حذف شود و یا اینکه بهبود یابد. یک نسخه بهبود یافته آن به صورت زیر است:

</div>

```

// Add a new data point (count >= 0).
// For the next minute, MinuteCount() will be larger by +count.
// For the next hour, HourCount() will be larger by +count.
void Add(int count);

```
<div dir='rtl'>

اکنون بیایید کامنت مربوط به MinuteCount() را در نظر بگیریم:

</div>

```

// Return the count over this minute
int MinuteCount();

```

<div dir='rtl'>

وقتی که از همکاران خود درباره معنای این کامنت سوال کردیم، دو تفسیر متناقض بیان کردند:

1. برگرداندن شمارش در طول دقیقه فعلی مثلا 12:13pm
2. برگرداندن شمارش در طول ۶۰ ثانیه گذشته، بدون در نظر گرفتن مرزهای دقیقه و ساعت.

تعبیر دوم چیزی است که در واقع عمل می‌کند. بنابراین بیایید این ابهام را با زبانی دقیق‌تر و با جزئیات بیشتر پاکسازی کنیم:

</div>

```

// Return the accumulated count over the past 60 seconds.
int MinuteCount();

```
<div dir='rtl'>

به طور مشابه، ما باید کامنت مربوط به HourCount() را نیز بهبود دهیم.

</div>

```

// Track the cumulative counts over the past minute and over the past hour.
// Useful, for example, to track recent bandwidth usage.
class MinuteHourCounter {
    // Add a new data point (count >= 0).
    // For the next minute, MinuteCount() will be larger by +count.
    // For the next hour, HourCount() will be larger by +count.
    void Add(int count);
    // Return the accumulated count over the past 60 seconds.
    int MinuteCount();
    // Return the accumulated count over the past 3600 seconds.
    int HourCount();
};

```

<div dir='rtl'>

برای کوتاه‌تر بودن کدها، ما از این پس کامنت‌ها را از لیست کدها خارج خواهیم کرد.

## به دست آوردن یک چشم انداز بیرونی

احتمالا می‌دانید که قبلا مواردی وجود داشت که ما آن‌ها را از همکاران خود درخواست می‌کردیم. درخواست یک چشم انداز بیرونی یک راه عالی جهت تست این مورد است که آیا کد شما کاربر-پسند1 است یا نه؟ سعی کنید که در اولین برداشت‌های همکارانتان راحت باشید، زیرا افراد دیگر ممکن است به همان نتایج برسند و احتمالا شما نیز شش ماه بعد جزو همان «افراد دیگر» باشید.

## تلاش شماره ۱: یک راه حل ساده و بی تکلف

بیایید از این بحث بگذریم و به سراغ حل مسئله برویم. ما با یک راه حل مستقیم شروع می‌کنیم: فقط لیستی از برچسب زمانی2 Event‌ها را نگه‌داری کنید.

</div>

```

class MinuteHourCounter {
    struct Event {
        Event(int count, time_t time) : count(count), time(time) {}
        int count;
        time_t time;
    };
    list<Event> events;
  public:
    void Add(int count) {
        events.push_back(Event(count, time()));
    }
    ...
};

```

<div dir='rtl'>

سپس می‌توانیم جدیدترین رخدادها را در صورت لزوم محاسبه کنیم:

</div>

```

class MinuteHourCounter {
    ...
    int MinuteCount() {
        int count = 0;
        const time_t now_secs = time();
        for (list<Event>::reverse_iterator i = events.rbegin();
             i != events.rend() && i->time > now_secs - 60; ++i) {
            count += i->count;
        }
        return count;
    }
    int HourCount() {
        int count = 0;
        const time_t now_secs = time();
        for (list<Event>::reverse_iterator i = events.rbegin();
             i != events.rend() && i->time > now_secs - 3600; ++i) {
            count += i->count;
        }
        return count;
    }
};

```
<div dir='rtl'>

## آیا درک این کد آسان است؟

اگرچه این راه حل صحیح است، اما خالی از اشکالات خوانایی نیست:

* **حلقه‌های for کمی سنگین و پیچیده هستند.** اکثر افراد هنگام خواندن این بخش از کد، به طور قابل توجهی از سرعت خواندنشان کم می‌شود(آن‌ها حداقل باید اطمینان حاصل کنند که اشکالی وجود ندارد).
* **عبارت MinuteCount() و HourCount() تقریبا یکسان هستند.** اگر بتوانند کدهای تکرار شده را به اشتراک بگذارند، باعث می‌شود این کد کوچک‌تر شود. به طور خاص این جزئیات مهم‌اند، زیرا افزونگی کد نسبتا پیچیده است(بهتر است همه کدهای دشوار را در یک مکان محدود کنید).

## نسخه ساده‌تر از نظر خوانایی

کد مربوط به MinuteCount() و HourCount() تنها در یک ثابت متفاوت هستند (یعنی 60 در مقابل 3600). واضح است که بازسازی آن‌ها به این صورت خواهد بود که یک متد کمکی برای مدیریت هر دو آن‌ها معرفی کنیم:

</div>

```

class MinuteHourCounter {
    list<Event> events;
    int CountSince(time_t cutoff) {
        int count = 0;
        for (list<Event>::reverse_iterator rit = events.rbegin();
             rit != events.rend(); ++rit) {
            if (rit->time <= cutoff) {
                break;
            }
            count += rit->count;
        }
        return count;
    }
  public:
    void Add(int count) {
        events.push_back(Event(count, time()));
    }
    int MinuteCount() {
        return CountSince(time() - 60);
    }
    int HourCount() {
        return CountSince(time() - 3600);
    }
};

```

<div dir='rtl'>

موارد قابل اشاره در کد جدید عبارتند از:

اول، توجه کنید که CountSince() یک پارامتر دقیق cutoff را به جای یک مقدار نسبی secs_ago دریافت می‌کند(یعنی 60 یا 3600). هر چند در هر صورت کار خواهد کرد، اما شیوه CountSince() کار ساده‌تری انجام می‌دهد.

دوم، ما متغیر تکرار شونده را از i به rit تغییر نام دادیم. نام i معمولا برای شاخص‌های عدد صحیح استفاده می‌شود. ما در استفاده از آن نام که معمولا برای تکرار کننده‌ها است اندیشیدیم، اما در این مورد یک تکرار کننده معکوس داریم و این واقعیت، برای صحت کد بسیار مهم است. با داشتن یک نام متغیر که با پیشوند r شروع می‌شود، یک قرینه سازی ساده را برای دستوراتی شبیه rit != events.rend() اضافه می‌کنیم.

در نهایت، ما شرط rit->time <= cutoff را از حلقه for استخراج کرده و آن را با یک دستور if جداگانه ساختیم. دلیل این کار این بود که حلقه‌های سنتی for به شکل for(begin; end; advance) راحت‌تر خوانده می‌شوند. خواننده می‌تواند بلافاصله آن را درک کند که به معنی «همه عناصر را پشت سر بگذار» است و به تفکر بیشتری نیاز ندارد.

## مشکلات کارایی

اگرچه ما ظاهر کد را بهبود دادیم، ولی این طراحی دو مشکل جدی در کارایی دارد:

1. فقط در حال رشد کردن است.

این کلاس همه رخدادهایی که تا کنون دیده شده را نگه‌داری می‌کند. این کار یک مقدار بی حد و حصر از حافظه را استفاده می‌کند! در حالت ایده‌آل، MinuteHourCounter باید به صورت خودکار رخدادهایی که قدیمی‌تر از یک ساعت هستند را حذف کند، زیرا دیگر به آن‌ها نیازی نیست.

2. موارد MinuteCount() و HourCount() خیلی کند هستند.

متد CountSince() دارای سرعتی برابر O(n) است که n تعداد نقاط داده1 در پنجره زمانی مربوطه است. تصور کنید یک سرور با حداکثر کارآیی، تابع Add() را هزاران مرتبه در هر ثانیه فراخوانی کند. هر فراخوانی HourCount() باید از طریق یک میلیون نقاط داده محاسبه شود! در حالت ایده‌آل، MinuteHourCounter باید متغیرهای minute_count و hour_count را به صورت جداگانه به گونه‌ای نگه داری کند که با هر فراخوانی برای Add() به‌روزرسانی شده باشند.

## تلاش شماره ۲: طراحی تسمه نقاله1

ما به یک طراحی نیاز داریم که این دو مشکل را حل کند:

1. حذف داده‌ای که ما دیگر به آن نیازی نداریم.
2. نگه‌داری به روز رسانی‌های مجموع minute_count و hour_count که از قبل محاسبه شده‌اند.

در اینجا نحوه انجام این کار را نشان داده‌ایم:

ما از لیست خود مانند یک تسمه نقاله استفاده می‌کنیم یعنی زمانی که داده جدید به یک انتها می‌رسد، آن را به مجموع خود اضافه کرده و زمانی که داده خیلی قدیمی باشد، آن داده از انتهای دیگر «سقوط کرده» و ما آن را از مجموع خود کم می‌کنیم.
چندین راه برای پیاده سازی این طراحی تسمه نقاله وجود دارد. یک راه این است که دو لیست مستقل نگه‌داری کنید، یکی برای رخدادها در یک دقیقه گذشته و یکی هم برای رخدادها در یک ساعت گذشته. حال وقتی که یک رخداد جدید وارد شد، یک کپی به هر دو لیست اضافه کنید.

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/15-Designing-and-Implementing-a-Minute-Hour-Counter/img-15-3.png" />
</p>


این روش بسیار ساده، اما ناکارآمد است زیرا از هر رخداد دو کپی ایجاد می‌کند. 

راه دیگر این است که دو لیست نگه‌داری کنیم که ابتدا رخدادها وارد لیست اول شده(«رخدادهای یک دقیقه آخر») و سپس آن‌ها وارد لیست دوم شوند(رخدادهای یک ساعت آخر اما نه «رخدادهای یک دقیقه آخر»).

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/15-Designing-and-Implementing-a-Minute-Hour-Counter/img-15-4.png" />
</p>


این طراحی تسمه نقاله دو مرحله‌ای به نظر کارآیی بیشتری دارد، بنابراین اجازه دهید این را پیاده سازی کنیم.

## پیاده سازی طراحی تسمه نقاله دو مرحله‌ای

بیایید با لیست کردن همه اعضای کلاس خود، شروع کنیم:

</div>

```

class MinuteHourCounter {
    list<Event> minute_events;
    list<Event> hour_events;  // only contains elements NOT in minute_events
    int minute_count;
    int hour_count;  // counts ALL events over past hour, including past minute
};

```
<div dir='rtl'>

معمای طراحی تسمه نقاله این است که می‌تواند با گذشت زمان رخدادها را جا به جا1 کند، بنابراین رخدادها از minute_events به hour_events حرکت داده شده و minute_count و hour_count به ترتیب به‌روزرسانی می‌شوند. برای انجام این کار ما یک متد کمکی به نام ShiftOldEvents() خواهیم ساخت. هنگامی که این متد را داشته باشیم، پیاده سازی بقیه کلاس آسان است:

</div>

```

void Add(int count) {
    const time_t now_secs = time();
    ShiftOldEvents(now_secs);
    // Feed into the minute list (not into the hour list--that will happen later)
    minute_events.push_back(Event(count, now_secs));
    minute_count += count;
    hour_count += count;
}
 
int MinuteCount() {
    ShiftOldEvents(time());
    return minute_count;
}
int HourCount() {
    ShiftOldEvents(time());
    return hour_count;
}

```
<div dir='rtl'>

به طور واضح، ما همه کارهای کثیف را در تابع ShiftOldEvents() به تعویق انداختیم:

</div>

```

//Find and delete old events, and decrease hour_count and minute_count accordingly.
void ShiftOldEvents(time_t now_secs) {
    const int minute_ago = now_secs - 60;
    const int hour_ago = now_secs - 3600;
    // Move events more than one minute old from 'minute_events' into 'hour_events'
    // (Events older than one hour will be removed in the second loop.)
    while (!minute_events.empty() && minute_events.front().time <= minute_ago) {
        hour_events.push_back(minute_events.front());
        minute_count -= minute_events.front().count;
        minute_events.pop_front();
    }
    // Remove events more than one hour old from 'hour_events'
    while (!hour_events.empty() && hour_events.front().time <= hour_ago) {
        hour_count -= hour_events.front().count;
        hour_events.pop_front();
    }
}

```
<div dir='rtl'>

## آیا کار ما تمام شده است؟

ما دو مشکل کارآیی را که قبلا اشاره شده بود حل کردیم و راه حل ما به خوبی کار می‌کند. برای بسیاری از برنامه‌ها، این راه حل به اندازه کافی مناسب خواهد بود. اما همچنان برخی از نواقص باقی مانده‌اند. 

اول اینکه، طراحی اصلا انعطاف‌پذیر نیست. فرض کنید ما بخواهیم شمارش بیش از ۲۴ ساعت گذشته را نگه‌داری کنیم. این امر مستلزم ایجاد تغییرات زیادی در کد است. همان گونه که احتمالا متوجه شده‌اید، ShiftOldEvents() یک تابع بسیار متراکم با تعامل دقیق بین داده‌های دقیقه و ساعت است.

دوم، این کلاس دارای مقدار حافظه بسیار بزرگ است. فرض کنید شما یک سرور با ترافیک خیلی زیاد که تابع Add() را ۱۰۰ بار در هر ثانیه فراخوانی می‌کند دارید. از آنجا که ما برای همه داده‌ها در یک ساعت گذشته صبر می‌کنیم، این کد در نهایت باید به حدود 5MB حافظه نیاز داشته باشد.

به طور کلی، هرچه Add() بیشتر فراخوانی شده باشد، ما حافظه بیشتری استفاده می‌کنیم. در یک محیط تولید محصول، کتابخانه‌هایی که از مقدار حافظه زیادی به صورت پیش‌بینی نشده استفاده می‌کنند، خوب نیستند. در حالت ایده‌آل، MinuteHourCounter باید مقدار ثابتی از حافظه را بدون توجه به تعداد فراخوانی‌های Add() استفاده کند.

## تلاش شماره ۳: طراحی یک Time-Bucketed

شاید متوجه نشده باشید، اما هر دو پیاده سازی قبلی یک باگ کوچک داشتند. ما از time_t برای ذخیره‌سازی برچسب زمانی1 استفاده کردیم، که تعداد صحیح اعداد از ثانیه‌ها را ذخیره می‌کرد. بخاطر این گرد کردن، در واقع MinuteCount()، بسته به اینکه دقیقا چه زمانی آن را صدا می‌زنید، چیزی بین 59 و 60 ثانیه را بر می‌گرداند.

به عنوان مثال، اگر یک رخداد در time = 0.99 اتفاق افتد، این زمان به ثانیه t=0 گرد خواهد شد و اگر شما MinuteCount() را در ثانیه time = 60.1 صدا بزنید، این تابع، مقدار مجموع را برای رخدادهای t=1,2,3,...60 برخواهد گرداند. بنابراین اولین رخداد، از دست خواهد رفت، حتی اگر از نظر فنی کمتر از یک دقیقه قبل باشد.

به طور متوسط، MinuteCount() داده‌ای به ارزش  59.5ثانیه و HourCount() داده‌ای به ارزش  3599.5 را بر می‌گرداند(یک خطای ناچیز).
 
ما می‌توانیم همه این‌ها را با استفاده از یک زمان با granularity2 فرعی تصحیح کنیم. اما جالب این است که، اکثر برنامه‌هایی که از یک MinuteHourCounter استفاده می‌کنند در درجه اول به این میزان از دقت نیازی ندارند. ما از این حقیقت برای طراحی جدیدی از MinuteHourCounter بهره‌برداری خواهیم کرد که خیلی سریعتر و از فضای کمتری استفاده می‌کند. این یک trade-off بین دقت3 و کارآیی4 است که ارزش پرداختن به آن را دارد.

ایده اصلی این است که همه رخدادها را داخل یک پنجره زمانی کوچک در یک سطل5 قرار داده و آن رخدادها را با یک مقدارِ مجموع تکی خلاصه کنیم. به عنوان نمونه، رخدادهای یک دقیقه گذشته را می‌توان داخل ۶۰ سطل مجزا، با عرض یک ثانیه و رخدادهای یک ساعت گذشته را در ۶۰ سطل مجزا، با عرض ۱ دقیقه‌ای، قرار داد.

<p align="center">
    <img src="https://github.com/Hossein52Hz/The-Art-Of-Readable-Code-Persian/blob/main/15-Designing-and-Implementing-a-Minute-Hour-Counter/img-15-5.png" />
</p>

همانطور که در شکل نشان داده شده است، با استفاده از سطل‌ها متد MinuteCount() و HourCount() به دقت 1 قسمت در هر 60 قسمت رسیده‌اند که قابل قبول است1.

در صورتی که برای حافظه با مقدار بزرگتر، به دقت بیشتری نیاز دارید، می‌توان از سطل‌های بیشتری برای تبادلات استفاده کرد. اما مهمترین چیز این است که این طراحی دارای یک مصرف حافظه قابل پیش‌بینی و تصحیح شده است.

## پیاده سازی طراحی Time-Bucketed

پیاده سازی این طراحی فقط با یک کلاس، تعداد زیادی کد پیچیده ایجاد می‌کند که مرور کردن آن‌ها  در ذهن سخت خواهد بود. در عوض، ما توصیه خود را از فصل ۱۱ دنبال می‌کنیم، یعنی «یک وظیفه در یک زمان» و کلاس‌های جداگانه‌ای برای مدیریت بخش‌های مختلف این مسئله ایجاد می‌کنیم.

برای شروع، بیایید یک کلاس جداگانه برای پیگیری شمارش‌ها برای یک مدت زمان واحد(مثلا حداقل یک ساعت) ایجاد کنیم. ما نام این کلاس را TrailingBucketCounter می‌گزاریم. این در اصل یک نسخه عمومی از MinuteHourCounter است که تنها یک واحد زمانی را مدیریت می‌کند. Interface به شکل زیر خواهد بود:

</div>

```

// A class that keeps counts for the past N buckets of time.
class TrailingBucketCounter {
    public:
      // Example: TrailingBucketCounter(30, 60) tracks the last 30 minute-buckets of time.
      TrailingBucketCounter(int num_buckets, int secs_per_bucket);
      void Add(int count, time_t now);
      // Return the total count over the last num_buckets worth of time
      int TrailingCount(time_t now);
  };

```

<div dir='rtl'>

احتمالا نگرانید که چرا Add() و TrailingCount() به زمان فعلی(time_t) به عنوان یک آرگومان نیاز دارند. نگران نباشید! اگر این متدها فقط time() فعلی خودشان را محاسبه می‌کردند، ساده‌تر نبود؟

اگرچه ممکن است عجیب به نظر برسد، صرف نظر کردن از زمان فعلی، چندین مزیت دارد. اول اینکه TrailingBucketCounter که یک کلاس بدون زمان است را ایجاد می‌کند، که برای تست ساده‌تر بوده و کمتر مستعد اشکال1 است. دوم اینکه، همه فراخوانی‌ها را برای time() در MinuteHourCounter نگه‌داری می‌کند. داشتن یک سیستم حساس به زمان، در صورتی که به شما کمک کند همه فراخوانی‌های time را در یک مکان به دست آورید، مفید است.

با فرض اینکه TrailingBucketCounter قبلا پیاده سازی شده، پیاده سازی MinuteHourCounter ساده خواهد بود:

</div>

```

class MinuteHourCounter {
    TrailingBucketCounter minute_counts;
    TrailingBucketCounter hour_counts;
  public:
    MinuteHourCounter() :
        minute_counts(/* num_buckets = */ 60, /* secs_per_bucket = */ 1),
        hour_counts(  /* num_buckets = */ 60, /* secs_per_bucket = */ 60) {
    }
    void Add(int count) {
        time_t now = time();
        minute_counts.Add(count, now);
        hour_counts.Add(count, now);
    }
    int MinuteCount() {
        time_t now = time();
        return minute_counts.TrailingCount(now);
    }
    int HourCount() {
        time_t now = time();
        return hour_counts.TrailingCount(now);
    }
};

```
<div dir='rtl'>

خوانایی این کد خیلی بیشتر بوده و انعطاف پذیری بیشتری نیز دارد. این کار ساده‌ای است که تعداد سطل‌ها را، جهت بهبود دقت افزایش دهیم، اما به یاد داشته باشید که مصرف حافظه افزایش پیدا می‌کند.

## پیاده سازی TrailingBucketCounter

اکنون تنها چیزی که برای پیاده سازی باقی مانده، کلاس TrailingBucketCounter است. یک بار دیگر، ما قصد داریم یک کلاس کمکی برای جداسازی مشکل بعدی ایجاد کنیم. یک ساختار داده به نام ConveyorQueue ایجاد خواهیم کرد که وظیفه‌اش سر و کار داشتن با شمارش‌ها و مجموع آن‌ها است. کلاس TrailingBucketCounter می‌تواند روی کار جابجا کردن ConveyorQueue مطابق با اینکه چه مقدار زمان سپری شده است، تمرکز کند.

در ادامه رابط ConveyorQueue را مشاهده می‌کنید:

</div>

```

// A queue with a maximum number of slots, where old data "falls off" the end.
class ConveyorQueue {
    ConveyorQueue(int max_items);
        // Increment the value at the back of the queue.
        void AddToBack(int count);
        // Each value in the queue is shifted forward by 'num_shifted'.
        // New items are initialized to 0.
        // Oldest items will be removed so there are <= max_items.
        void Shift(int num_shifted);
        // Return the total value of all items currently in the queue.
        int TotalSum();
    };

```
<div dir='rtl'>

فرض کنید این کلاس پیاده سازی شده بود، نگاه کنید که پیاده سازی TrailingBucketCounter چقدر آسان است:

</div>

```

class TrailingBucketCounter {
    ConveyorQueue buckets;
    const int secs_per_bucket;
    time_t last_update_time;  // the last time Update() was called
    // Calculate how many buckets of time have passed and Shift() accordingly.
    void Update(time_t now) {
        int current_bucket = now / secs_per_bucket;
        int last_update_bucket = last_update_time / secs_per_bucket;
   
        buckets.Shift(current_bucket - last_update_bucket);
        last_update_time = now;
    }
  public:
    TrailingBucketCounter(int num_buckets, int secs_per_bucket) :
        buckets(num_buckets),
        secs_per_bucket(secs_per_bucket) {
    }
    void Add(int count, time_t now) {
        Update(now);
        buckets.AddToBack(count);
    }
    int TrailingCount(time_t now) {
        Update(now);
        return buckets.TotalSum();
    }
};

```
<div dir='rtl'>

کار شکستن کد به دو کلاس TrailingBucketCounter و ConveyorQueue نمونه دیگری از بحثی است که در فصل ۱۱ داشتیم، یعنی یک وظیفه در یک زمان. همچنین می‌توانستیم بدون ConveyorQueue و با پیاده سازی مستقیم همه چیز داخل TrailingBucketCounter، کار مد نظر خود را انجام دهیم. اما با شکستن کد به دو کلاس، هضم کد آسان‌تر است. 

</div>

```

// A queue with a maximum number of slots, where old data gets shifted off the end.
class ConveyorQueue {
    queue<int> q;
    int max_items;
    int total_sum;  // sum of all items in q
  public:
    ConveyorQueue(int max_items) : max_items(max_items), total_sum(0) {
    }
    int TotalSum() {
        return total_sum;
    }
    void Shift(int num_shifted) {
        // In case too many items shifted, just clear the queue.
        if (num_shifted >= max_items) {
            q = queue<int>();  // clear the queue
            total_sum = 0;
            return;
        }
        // Push all the needed zeros.
        while (num_shifted > 0) {
            q.push(0);
            num_shifted--;
        }
        // Let all the excess items fall off.
        while (q.size() > max_items) {
            total_sum -= q.front();
            q.pop();
        }
    }
    void AddToBack(int count) {
        if (q.empty()) Shift(1);  // Make sure q has at least 1 item.
        q.back() += count;
        total_sum += count;
    }
};

```
<div dir='rtl'>

اکنون کار ما تمام شده است! ما یک MinuteHourCounter داریم که سریعتر و از نظر مصرف حافظه کارا‌یی بالاتری دارد، علاوه بر این، TrailingBucketCounter انعطاف‌پذیرتر است و در نتیجه به راحتی قابل استفاده مجدد است. به عنوان نمونه خیلی ساده خواهد بود که یک RecentCounter تطبیق پذیرتر1 که بتواند دامنه وسیعی از فاصله‌ها2 را شمارش کند، ساخته شود(مانند آخرین روز یا آخرین ده دقیقه).

## مقایسه سه راه حل 

بیایید راه‌حل‌هایی که در این فصل ارائه شد را با هم مقایسه کنیم. جدول زیر اندازه کد و آمار کارآیی را با فرض یک مورد کاربردِ ترافیک بالا از ۱۰۰ مرتبه استفاده از تابع Add() در هر ثانیه(100 Add()/sec) را نشان می‌دهد:

| خطا در HourCount() | مصرف حافظه                | هزینه به ازای هر HourCount()      | تعداد خطوط کد | راه حل                            |
|--------------------|---------------------------|-----------------------------------|---------------|-----------------------------------|
| 1 part per 3600    | unbounded                 | O(#events-per-hour) (3.6 million) | 33            | Naive solution                    |
| 1 part per 3600    | O(#events-per-hour) (5MB) | O(1)                              | 55            | Conveyor belt design              |
| 1 part per 60      | O(#buckets) (500 bytes)   | O(1)                              | 98            | Time-bucketed design (60 buckets) |


توجه کنید که مقدار کل کد، برای راه حل‌های نهایی سه کلاس بیشتر از هر یک از موارد دیگر است. با این حال کارآیی بسیار بهتر و طراحی آن انعطاف پذیری بیشتری خواهد داشت. همچنین، خواندن هر کلاس به صورت جداگانه راحت‌تر است. این همیشه یک تغییر مثبت است: داشتن ۱۰۰ خط که همه آن‌ها برای خواندن ساده هستند، بهتر از ۵۰ خطی است که خواندن‌شان ساده نیست. گاهی اوقات، شکستن یک مشکل به چندین کلاس می‌تواند پیچیدگی بین کلاسی ایجاد کند(در صورتی که راه حل تک کلاسی چنین کاری نمی‌کند). در این حالت، یک زنجیره خطی ساده برای عبور از یک کلاس به کلاس بعدی وجود دارد و فقط یکی از کلاس‌ها در معرض دید کاربر نهایی قرار می‌گیرد. به طور کلی مزیت شکستن این مسئله به زیرمسئله‌ها باعث یک پیروزی است.

## خلاصه فصل

بیایید مراحلی را که برای رسیدن به طرح نهایی MinuteHourCounter طی کردیم مرور کنیم. این فرآیند از چگونگی تکامل قسمت‌های مختلف کد به دست آمده است:

ابتدا ما با کدنویسی یک راه حل ساده و بی تکلف شروع کردیم. این به ما کمک کرد که دو مورد از  تغییرات طراحی را متوجه شویم: سرعت و مصرف حافظه.

در مرحله بعد، ما یک طرح تسمه نقاله را امتحان کردیم. این طراحی سرعت و مصرف حافظه را بهبود بخشید، اما هنوز به اندازه کافی برای کاربردهایی با کارآیی عالی مناسب نبود. همچنین این طراحی انعطاف پذیر نبود و تطبیق پذیری کد برای مدیریت دیگر فواصل زمانی نیازمند کار بسیاری بود.

طراحی نهایی ما مشکلات قبلی را با شکستن موارد در زیرمسئله‌ها حل کرد. در اینجا سه کلاس ایجاد شده به ترتیب پایین به بالا و زیرمسئله‌هایی که هر کدام حل کردند را داریم:

ConveyorQueue

حداکثر طول صف است که می‌تواند جابجا شود و مجموع کل را نگه‌داری کند.

TrailingBucketCounter

ConveyorQueue را با توجه به مدت زمان سپری شده و نگه‌داری چندین فاصله زمانی تکی(آخرین) با دقتی خاص، جابجا می‌کند.

MinuteHourCounter

به سادگی شامل دو TrailingBucketCounters است. یکی برای شمارش دقیقه و دیگری برای شمارش ساعت.

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

