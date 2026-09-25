# جلسه 25: Registers، Shift Registers، Serial Adder و Ripple Counters

> منبع: logiccircuit-hessabi-session-25.mp4 -> logiccircuit-hessabi-session-25.txt

این جلسه (یکی مانده به آخر) درباره register و shift register شروع می‌شود، به serial adder و universal shift register می‌رسد و سپس وارد بحث counter (به‌خصوص asynchronous / ripple counter) می‌شود و در دقایق پایانی نگاهی کوتاه به synchronous counter دارد.

---

## 📋 سرفصل این جلسه (Topics)

- پیاده‌سازی یک register از روی state table با D flip-flop (مثال حل‌شده با K-map)
- ساختار داخلی register با parallel load به‌کمک multiplexer در ورودی هر flip-flop
- روش‌های حفظ اطلاعات (hold): نگه‌داشتن load، ثابت‌نگه‌داشتن ورودی، و clock gating
- مفهوم clock gating، clock skew و مشکلات آن
- مدل shift register (ثبات انتقالی): serial in / serial out و تحلیل timing آن
- انواع shift register: SISO، SIPO، PISO، PIPO
- انتقال سریال بین دو register با shift control، و عمل rotate با برگرداندن خروجی به ورودی
- مدل serial adder با full adder و یک D flip-flop برای carry
- مدل کردن serial adder به‌صورت sequential circuit با یک state flip-flop برای carry
- بحث race / clock skew در serial adder و اینکه چرا مشکل ایجاد نمی‌شود
- مدل universal shift register چهار بیتی (nochange، shift right، shift left، parallel load) با MUX
- طراحی T flip-flop با استفاده از D flip-flop (مثال حل‌شده)
- تعریف synchronous در برابر asynchronous / ripple counter
- طراحی four bit binary ripple counter با T flip-flop و up counter / down counter
- نام‌های دیگر ripple counter: modulo 16، divide by 16، چهار stage
- مدل BCD ripple counter (شمارش 0 تا 9) با NAND و reset
- مدل three decade BCD counter (شمارش تا 999)
- مقدمه synchronous counter با count enable
- اعلانات پایان ترم (جلسه بعد، امتحان، کلاس رفع اشکال)

---

## 🔑 مفاهیم و تعاریف کلیدی (Key concepts)

- **Register / ثبات**: مجموعه‌ای از flip-flopها (طبق transcript معمولاً از نوع D) که به‌صورت یک واحد در نظر گرفته می‌شوند. کلاک همه آن‌ها مشترک است و برای هر flip-flop کلاک جدا گذاشته نمی‌شود.
- **Load / بارگزاری**: نوشتن یک اطلاعات جدید داخل register. وقتی سیگنال load برابر یک باشد، ورودی‌ها داخل register نوشته می‌شوند.
- **Hold / حفظ اطلاعات**: نگه‌داشتن محتوای register مستقل از ورودی‌ها. طبق transcript «حفظ اطلاعات» یعنی مستقل از ورودی‌ها اطلاعات باقی بماند.
- **Clock gating**: قطع کردن کلاک با AND کردن سیگنال کلاک با یک سیگنال کنترل، تا وقتی کلاک به register نرسد اطلاعات حفظ شود. روش خوب و پرکاربرد است ولی ایراد دارد (کلاک باید همزمان به همه flip-flopها برسد).
- **Clock skew / انحراف کلاک**: دیرتر رسیدن کلاک به یک register نسبت به بقیه مدار (طبق transcript «سکیو یعنی انحراف»). می‌تواند باعث شود اطلاعات درست load نشود. توضیح کامل به «درس‌های بعد» موکول شد.
- **Shift register / ثبات انتقالی**: register با قابلیت انتقال (shift) اطلاعات به سمت راست یا چپ. در هر لبه فعال کلاک، محتوای هر flip-flop به flip-flop مجاور منتقل می‌شود، و این انتقال‌ها همزمان رخ می‌دهند.
- **Serial input (SI) / serial output (SO)**: ورودی و خروجی سری (متوالی) shift register. طبق transcript خروجی سری با SO یا «سای اس او» (یعنی SISO) نشان داده شد.
- **Unidirectional / یک‌طرفه**: shift register که فقط به یک سمت shift می‌کند.
- **Bidirectional / دوطرفه**: shift register که می‌تواند به دلخواه به راست یا چپ shift کند (در هر کلاک فقط یک جهت).
- **انواع shift register از نظر ورودی/خروجی**: SISO (serial in serial out)، SIPO (serial in parallel out)، PISO (parallel in serial out)، PIPO (parallel in parallel out). PIPO یعنی همه ورودی‌ها همزمان load می‌شوند و همه خروجی flip-flopها همزمان در دسترس‌اند.
- **Rotate / چرخش**: وقتی serial output دوباره به serial input همان shift register برگردانده شود، عمل shift تبدیل به rotate می‌شود و بیت خارج‌شده از بین نمی‌رود بلکه دوباره وارد ابتدا می‌شود. طبق transcript «روتیت همون شیفته» با برگشت بیت آخر به خودش.
- **Serial adder**: جمع‌کننده که بیت‌ها را یک‌به‌یک (از LSB) در کلاک‌های متوالی جمع می‌کند، به‌جای اینکه n تا full adder به‌صورت parallel پشت‌سر هم بسته شود. مزیت آن کم شدن مساحت (area) است.
- **Counter / شمارنده**: از یک سری flip-flop ساخته می‌شود؛ دو نوع: synchronous (همگام) و asynchronous (ناهمگام).
- **Synchronous counter / همگام (sncron)**: کلاک همه flip-flopها به هم وصل است و همه همزمان تغییر می‌کنند.
- **Asynchronous counter / ناهمگام (آسنکرون) = ripple counter**: کلاک flip-flopها به هم وصل نیست؛ تغییر خروجی یک flip-flop به‌عنوان trigger (راه‌انداز) flip-flop بعدی استفاده می‌شود. به آن ripple counter (شمارنده موج‌گونه) هم می‌گویند.
- **n بیتی بودن counter**: برای شمارنده n بیتی به n عدد flip-flop نیاز است و از 0 تا (2^n - 1) می‌شمارد. مثلاً چهار بیتی: 2^4 = 16، از 0 تا 15.
- **نام‌های دیگر یک four bit binary ripple counter**: چهار stage (چهار طبقه)، modulo 16 (پیمانه شانزده)، divide by 16 (تقسیم بر شانزده). «پیمانه شانزده» یعنی از 0 تا 15 می‌شمارد و دوباره صفر می‌شود (نه «مبنای شانزده»؛ transcript این تفاوت را صریح توضیح می‌دهد).

---

## 📐 جبر بول، K-map و استدلال‌ها (Boolean algebra, K-maps, derivations)

### 1) پیاده‌سازی state table با D flip-flop (مثال ابتدای جلسه)

مدار دو flip-flop به نام A1 و A0 و یک ورودی X دارد (پس 2^3 = 8 حالت). state table (که سخنران نکشید) next state را در ستون next state و خروجی مدار Y را می‌دهد. چون از D flip-flop استفاده می‌شود، ورودی D هر flip-flop مساوی همان next state آن بیت است: D1 = A1+ و D0 = A0+.

از روی K-mapها (ورودی‌ها به‌صورت عمودی وارد شده‌اند)، نتایج زیر به‌دست آمد:

- **A1+ = D1 = A1 · X'**
- **A0+ = D0 = A0' · X + A0 · X' = A0 ⊕ X**، یعنی D0 = A0 XOR X.
- **Y = A0 · X**: فقط در دو خانه یک است و در آن دو خانه A0 = 1 و X = 1، پس Y = A0·X.

پیاده‌سازی مدار: یک register با دو D flip-flop. برای D1 یک AND gate (A1 و X') و برای D0 یک XOR gate (یکی از ورودی‌هایش A0 و دیگری X)، و خروجی Y = A0·X با یک AND gate. یک سیگنال کلاک مشترک برای کل register.

### 2) طراحی T flip-flop با استفاده از D flip-flop (مثال حل‌شده)

می‌خواهیم مداری بسازیم که مثل T flip-flop کار کند ولی فقط از D flip-flop استفاده کنیم. ورودی مدار T و خروجی Q. خروجی D flip-flop همان Q است.

state table از روی تعریف T flip-flop:

| Q | T | Q+ (next state) = D |
|---|---|---------------------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

- اگر T = 0: حالت عوض نمی‌شود (0 می‌ماند 0، 1 می‌ماند 1).
- اگر T = 1: toggle می‌شود (0 می‌شود 1، 1 می‌شود 0).

چون با D flip-flop می‌سازیم، Q+ همان D است. از جدول، D در دو جا یک است:

**D = Q' · T + Q · T' = Q ⊕ T**

D = Q'T + QT' دقیقاً تعریف XOR است، پس D = Q XOR T. مدار نهایی: یک D flip-flop با یک XOR gate در ورودی که Q (خروجی خودش) را با T اکس‌اور می‌کند. این همان T flip-flop است که «داخلش را با D flip-flop ساختیم».

### 3) استدلال up/down در ripple counter (جبر ترتیب گذرها)

- **A0 (LSB)**: در هر کلاک یک بار toggle می‌کند، پس فقط کافی است T آن به یک وصل شود و کلاک اصلی را بگیرد.
- در up counter با flip-flopهای T که با **لبه پایین‌رونده (falling edge)** کلاک می‌خورند: کلاک هر flip-flop از خروجی Q بیت کم‌ارزش‌ترِ کناری گرفته می‌شود. هر بیت وقتی از 1 به 0 می‌رود (این گذر یک falling edge است)، یک کلاک برای بیت سمت چپش (بیت با ارزش بیشتر) تولید می‌کند و آن toggle می‌شود. نتیجه: شمارش رو به بالا.
- **down counter، روش اول**: همان مدار ولی flip-flopها را با **لبه بالارونده (rising edge)** کلاک کنیم؛ در این حالت گذر 0 به 1 هر بیت باعث toggle بیت بعدی می‌شود و شمارنده رو به پایین می‌شمارد.
- **down counter، روش دوم**: کلاک‌ها را همان‌طور (falling edge) نگه داریم ولی به‌جای Q، خروجی Q' (Q not) را به کلاک flip-flop بعدی وصل کنیم. این هم down counter می‌دهد.
- اگر **هر دو** تغییر را با هم اعمال کنیم (هم لبه را عوض کنیم و هم Q' بدهیم)، «این دو اثر همدیگر را خنثی می‌کنند» و دوباره up counter می‌شود.

---

## 🧮 مثال‌ها و مدارهای حل‌شده (Worked examples / circuits)

### الف) Register با parallel load ساخته‌شده با MUX

یک ثبات چهار بیتی (four bit register) با ورودی parallel load. ورودی‌ها I0, I1, I2, I3 و خروجی‌ها A0, A1, A2, A3 (طبق transcript «آی» و «ای»). سیگنال load و سیگنال کلاک هم دارد.

- در ورودی هر D flip-flop یک multiplexer قرار دارد که با سیگنال load به‌عنوان خط select کنترل می‌شود. ساختار multiplexer از دو AND gate (یکی با load و یکی با load' که یک NOT در مسیر است) تشکیل شده که «چهار بار تکرار شده» است.
- **اگر load = 1**: ورودی‌های I3 تا I0 داخل flip-flopها نوشته (load) می‌شوند، یعنی A = I.
- **اگر load = 0**: خط دیگر MUX که از خروجی خود flip-flop گرفته شده انتخاب می‌شود، پس هر flip-flop مقدار قبلی خودش را دوباره می‌نویسد و اطلاعات حفظ می‌شود (next state = present state).

**روش‌های حفظ اطلاعات (hold) که در transcript مطرح شد:**
1. سیگنال load را روی صفر نگه داریم؛ هرچقدر هم کلاک بخورد، همان خروجی‌ها دوباره در خودشان نوشته می‌شوند.
2. ورودی‌ها را ثابت نگه داریم در حالی که load = 1؛ ولی transcript صریحاً می‌گوید این روش «معقول نیست» چون حفظ اطلاعات باید مستقل از ورودی‌ها باشد.
3. **Clock gating**: کلاک را با یک سیگنال کنترل AND کنیم (مثلاً clk1 = clock · control). با صفر نگه‌داشتن سیگنال کنترل، هر کلاکی بیاید به این register منتقل نمی‌شود و اطلاعات حفظ می‌شود. ایراد آن clock skew است (کلاک این register کمی دیرتر می‌رسد و ممکن است اطلاعات درست load نشود).

### ب) Shift register و تحلیل timing (SISO)

یک shift register چهار‌بیتی با flip-flopهایی که با **لبه بالارونده** کلاک می‌خورند. یک ورودی serial input (SI) و یک خروجی serial output (SO). در هر لبه بالارونده، اطلاعات هر flip-flop به flip-flop سمت راست منتقل می‌شود و همه این انتقال‌ها همزمان رخ می‌دهند (نه زنجیره‌ای).

مثال timing:
- SI ابتدا 0 است، سپس یک می‌شود، «مثلاً دو تا کلاک یک می‌ماند»، بعد دوباره 0 می‌شود.
- حالت اولیه داخل shift register نامعلوم است، پس همه خروجی‌ها را X (don't know) می‌گذاریم.
- کلاک اول: SI = 0، پس اولین flip-flop 0 می‌گیرد و بقیه یکی به راست shift می‌کنند (هنوز X).
- کلاک دوم: SI = 1، از سمت چپ 1 وارد می‌شود و 0 قبلی یکی به راست می‌رود.
- کلاک سوم: SI = 1 دوباره، یک 1 دیگر وارد می‌شود.
- کلاک چهارم: SI = 0، از سمت چپ 0 وارد می‌شود و بقیه یکی به راست shift می‌کنند.

بعد از چهار بار shift، هر مقدار serial ورودی پس از چهار کلاک به خروجی SO می‌رسد (در این shift register چهار‌بیتی).

اگر به‌جای اینکه فقط خروجی flip-flop آخر بیرون داده شود، تمام چهار خروجی flip-flopها به پایه‌های IC بیایند، آن‌گاه Serial In / Parallel Out (SIPO) می‌شود. ترکیب‌های دیگر: SISO، SIPO، PISO، PIPO.

### ج) انتقال سریال بین دو register و rotate

دو register A و B (هرکدام می‌توانند 4 یا 8 بیتی باشند). خروجی serial ثبات A به serial input ثبات B وصل شده است. کلاک هر دو با یک **clock gating** ساخته شده: کلاک عادی AND شده با یک سیگنال shift control.
- اگر shift control = 0: کلاک هیچ‌کدام نمی‌خورد و اتفاقی نمی‌افتد.
- اگر shift control = 1: در هر کلاک هر دو یکی به سمت راست shift می‌کنند.

اگر اینها چهار بیتی باشند، بعد از چهار بار کلاک تمام اطلاعات shift register A داخل shift register B نوشته می‌شود.

**عمل rotate**: در این مثال خروجی serial ثبات A به serial input خودِ A هم برگردانده شده. پس به‌جای shift ساده، عمل rotate انجام می‌شود: بیتی که از انتها خارج می‌شود دوباره به ابتدای همان register وارد می‌شود و از بین نمی‌رود.

**مثال عددی rotate:**
- استاد یک مقدار اولیه چهار بیتی برای هر یک از دو shift register (A و B) فرض می‌کند و رفتار کلاک‌به‌کلاک را دنبال می‌کند.
- از روایت کلاک‌به‌کلاک، بیت خروجی اولِ A برابر یک است و به‌دلیل rotate همان یک به ابتدای A برمی‌گردد؛ محتوای A رفتاری مطابق **1 0 1 1** دارد که به‌صورت راست‌گرد rotate می‌شود.

رفتار مدار روشن است: در هر کلاک بیت خروجی A هم وارد B می‌شود و هم (به‌دلیل rotate) به ابتدای خود A برمی‌گردد؛ B به‌صورت معمولی shift می‌کند و بیت خروجی‌اش دور ریخته می‌شود.

### د) Serial adder با full adder و D flip-flop

دو عدد n بیتی که داخل shift registerهای A و B هستند را بیت‌به‌بیت جمع می‌کنیم:
- یک full adder: یک ورودی از shift register A، یک ورودی از shift register B، و ورودی سوم (Z یا carry in) از یک D flip-flop.
- خروجی full adder: Sum و Carry out.
- **carry**: در ابتدا D flip-flop باید reset باشد (carry اولیه = 0). در هر کلاک، carry out داخل D flip-flop نوشته می‌شود تا در کلاک بعدی به‌عنوان carry in بیت با ارزش بیشتر استفاده شود.
- **Sum**: بیت‌به‌بیت تولید می‌شود و باید داخل یک shift register نوشته شود (چون سریال وارد می‌شود). طبق transcript می‌توان Sum را در خروجی یک shift register نوشت یا آن را به داخل یکی از همان register‌ها (مثلاً A) برگرداند، چون اطلاعات A همان‌طور که shift می‌شود بیرون می‌آید و از سمت چپ می‌توان حاصل جمع جدید را وارد کرد.
- شروع از LSB (بیت کم‌ارزش‌تر).
- کلاک این مدار: کلاک عادی AND شده با یک shift control (شبیه clock gating قبلی). فقط وقتی shift انجام شود کلاک به carry flip-flop هم می‌رسد؛ اگر shift انجام نشود، carry قبلی نباید تغییر کند.

**راه‌اندازی وقتی اعداد از قبل داخل A و B نیستند:** ابتدا D flip-flop و register A را reset می‌کنیم (پس A تماماً صفر و carry اولیه صفر). عدد اول را از طریق serial input ثبات B بیت‌به‌بیت وارد می‌کنیم؛ چون A صفر است، full adder عدد B را با صفر جمع می‌کند و مستقیماً به A منتقل می‌کند. پس از چند کلاک (به تعداد بیت‌ها)، عدد اول داخل A و عدد دوم داخل B قرار می‌گیرد و در این مدت carry همیشه صفر می‌ماند (چون با صفر جمع شده). سپس مرحله جمع واقعی شروع می‌شود.

**مراحل خلاصه serial adder (طبق transcript):**
1. reset کردن D flip-flop و register A (فرض: عدد اول در register B است).
2. انتقال عدد موجود در B به A از طریق full adder (چون A صفر است، انگار خود B منتقل می‌شود)، و همزمان load کردن عدد دوم به B از طریق serial input.
3. جمع بیت‌به‌بیت A با B (شروع از LSB) و نوشتن نتیجه داخل register A؛ در هر کلاک یک بیت.

### ه) Serial adder به‌عنوان sequential circuit (مدل حالت carry)

به‌جای طراحی بالا، serial adder را می‌توان به‌صورت یک مدار ترتیبی مدل کرد:
- **state**: یک flip-flop که carry فعلی را نگه می‌دارد (0 یا 1).
- **inputs**: دو بیت X و Y.
- **outputs**: Sum و همچنین carry بعدی (next state).

نمونه سطرهایی که transcript توصیف می‌کند:
- X = 0, Y = 0, carry = 0: Sum = 0، carry بعدی = 0.
- یکی از X,Y صفر و دیگری یک، carry = 0: Sum = 1، carry = 0.
- یکی صفر و دیگری یک، carry (present) = 1: Sum = 0، carry بعدی = 1 (طبق transcript «سام میشه صفر، کری میشه یک»).

برای full adder، Sum = X ⊕ Y ⊕ Cin و Cout = XY + Cin(X⊕Y). سطر سوم (X⊕Y = 1، Cin = 1): Sum = 1⊕1 = 0 و Cout = 0 + 1·1 = 1، پس این مقادیر سازگارند. state table و ساده‌سازی کامل آن در کتاب آمده و چون شکل عادی است کشیده نشد.

### و) Universal shift register چهار بیتی

یک shift register چهار بیتی PIPO (چهار parallel input و چهار parallel output). دو خط کنترل **S1 و S0** (که با ترکیب‌های 00، 01، 10، 11 چهار عملکرد را انتخاب می‌کنند). یک کلاک و یک clear' (active low، با صفر شدن clear می‌شود).

ورودی D هر flip-flop از یک multiplexer مستقل می‌آید و همه MUXها با همان S1 S0 کنترل می‌شوند. چهار عملکرد بر اساس S1 S0:

| S1 S0 | عملکرد | ورودی انتخاب‌شده MUX هر flip-flop |
|-------|--------|------------------------------------|
| 0 0 | **no change** (hold) | خط 0: خروجی خودِ همان flip-flop (An -> An) |
| 0 1 | **shift right** | خط 1: از flip-flop با ارزش بیشتر (An+1 -> An)؛ A3 از serial input for shift right |
| 1 0 | **shift left** | خط 2: از flip-flop با ارزش کمتر (An-1 -> An)؛ A0 از serial input for shift left |
| 1 1 | **parallel load** | خط 3: ورودی موازی (parallel input An) |

جزئیات طبق transcript:
- **00 (no change)**: خط 0 هر MUX از خروجی خود flip-flop گرفته شده، پس اطلاعات همان‌جا دوباره نوشته می‌شود و تغییری نمی‌کند.
- **01 (shift right)**: A3 داخل A2 نوشته می‌شود، A2 داخل A1، A1 داخل A0، و داخل A3 خودِ «serial input for shift right» نوشته می‌شود (چون MSB اول باید serial in بگیرد).
- **10 (shift left)**: A2 داخل A3، A1 داخل A2، A0 داخل A1، و داخل A0 خودِ «serial input for shift left» نوشته می‌شود (چون LSB باید از بیرون بیاید). در حالت 10 که shift left است، A0 همان بیتی است که serial input را می‌گیرد.
- **11 (parallel load)**: خط 3 هر MUX از parallel input گرفته شده و همه ورودی‌های موازی (A3 A2 A1 A0، چهار parallel input مستقل) همزمان در یک کلاک نوشته می‌شوند.

توجه: چون هرگز هم‌زمان shift left و shift right انجام نمی‌شود، دو خط serial input می‌توانند یک خط مشترک باشند.

---

## 🧠 روش‌ها و الگوریتم‌ها (Procedures / algorithms)

**طراحی یک sequential circuit / register از روی state table (روش کلی، همان روش جلسه قبل):**
1. برای هر flip-flop، next state آن بیت را از state table استخراج کن.
2. اگر از D flip-flop استفاده می‌کنی، D هر بیت = next state همان بیت.
3. برای هر D یک K-map بکش و ساده کن.
4. مدار ترکیبی ورودی flip-flopها را از عبارت‌های ساده‌شده بساز (با AND/OR/XOR یا با MUX).

**طراحی هر flip-flop با نوع دیگر (مثل T با D):**
1. state table مدار مطلوب (اینجا رفتار T) را بنویس: Q, input -> Q+.
2. Q+ را برابر D بگیر (چون D flip-flop استفاده می‌شود) و ساده کن -> D = Q ⊕ T.
3. transcript تأکید: لازم نیست حفظ کنیم؛ «هر flip-flop را می‌توان با هر نوع دیگر ساخت» و این یک مسئله طراحی flip-flop است.

**طراحی binary ripple counter (asynchronous):**
1. از T flip-flop استفاده کن (چون در ripple counter ساده‌تر است) و همه Tها را به یک (5 volt) وصل کن، پس هر flip-flop در هر کلاک خودش toggle می‌کند.
2. کلاک هر flip-flop را از خروجی flip-flop بیت با ارزش کمتر (کناری) بگیر.
3. با لبه falling: هر بار که یک بیت از 1 به 0 می‌رود، بیت سمت چپش toggle می‌شود -> up counter.
4. برای reset اولیه یک clear' (active low) به همه flip-flopها بده.

**تبدیل به BCD ripple counter (شمارش 0 تا 9):**
1. همان binary ripple counter را بگیر.
2. عدد 10 را با AND/NAND از بیت‌های مربوطه تشخیص بده (طبق transcript، در عدد ده Q4 و Q2 هر دو یک‌اند؛ آن‌ها را NAND کن).
3. وقتی به 10 رسید خروجی NAND صفر می‌شود و آن را به reset همه flip-flopها وصل کن، پس بلافاصله بعد از 9 به 0 برمی‌گردد.
4. روش جایگزین (فقط اشاره شد): وقتی به 9 رسید با کلاک بعدی همه را preset کنیم تا 15 شود و 15 بلافاصله صفر می‌شود.

---

## 📊 نمودارها (Diagrams: circuit / timing / state, described from audio)

- **Register با MUX در ورودی**: هر D flip-flop یک multiplexer دو‌ورودی در ورودی‌اش دارد؛ select آن سیگنال load است؛ یک ورودی MUX پارالل input (I) و ورودی دیگر خروجی خود flip-flop (feedback). این بلوک چهار بار برای یک register چهار‌بیتی تکرار شده است.
- **Shift register (SISO)**: زنجیره‌ای از D flip-flop که خروجی هرکدام به ورودی flip-flop سمت راست وصل است؛ یک serial input در چپ و یک serial output در راست، همه با یک کلاک لبه بالارونده.
- **Timing diagram shift register**: تغییرات فقط در لبه‌های بالارونده کلاک؛ مقادیر اولیه X (نامعلوم)؛ ورودی serial با الگوی 0،1،1،0 که یکی‌یکی به راست شیفت می‌کند.
- **دو register با clock gating و rotate**: خروجی A به serial input B و همچنین به serial input خود A برمی‌گردد (rotate)؛ کلاک هر دو = clock AND shift-control.
- **Serial adder**: یک full adder، دو ورودی از shift register A و B، ورودی سوم carry از یک D flip-flop؛ Sum به یک shift register، Cout به D flip-flop؛ کلاک = clock AND shift-control.
- **Universal shift register**: چهار D flip-flop، هرکدام با یک 4-to-1 MUX در ورودی؛ همه MUXها با S1 S0 آدرس‌دهی می‌شوند؛ خطوط 0..3 هر MUX به‌ترتیب: خودِ flip-flop (hold)، همسایه پرارزش (shift right)، همسایه کم‌ارزش (shift left)، parallel input.
- **Four bit binary ripple counter**: چهار flip-flop JK که J,K هرکدام به 5 volt وصل (پس T flip-flop)؛ کلاک flip-flop اول از کلاک اصلی، کلاک هر flip-flop بعدی از خروجی Q flip-flop قبلی؛ یک clear'.
- **Timing diagram ripple counter**: در هر لبه پایین‌رونده کلاک، Q0 toggle؛ هر جا Q0 از 1 به 0 می‌رود Q1 toggle؛ هر جا Q1 از 1 به 0 می‌رود Q2 toggle؛ هر جا Q2 از 1 به 0 می‌رود Q3 toggle. تأکید: این تغییرات ripple دارند (تأخیر پله‌ای)، برخلاف synchronous که همزمان‌اند. مثلاً از 15 (1111) به 0، اول بیت اول تغییر می‌کند، یک لحظه بعد بیت بعدی و ... .
- **فرکانس‌ها در ripple counter (تقسیم فرکانس)**: اگر کلاک فرکانس f داشته باشد، Q1 = f/2، Q2 = f/4، Q3 = f/8، Q4 = f/16؛ پس مدار فرکانس کلاک ورودی را تقسیم بر 16 می‌کند (نام divide by 16). مثال عددی: کلاک 1 MHz یعنی دوره 1 microsecond.
- **BCD ripple counter**: همان ripple counter با یک NAND روی Q4 و Q2 (بیت‌های عدد ده) که خروجی‌اش به reset همه flip-flopها می‌رود.
- **Three decade BCD counter**: سه بلوک BCD counter پشت‌سر هم؛ carry هر رقم (وقتی بیت وزن‌ ۸ آن رقم، یعنی Q4، از 1 به 0 می‌رود و رقم از 9 به 0 برمی‌گردد) به کلاک رقم بعدی می‌رود؛ می‌شمارد 0 تا 999 و بعد 0. این همان بیت وزن‌ ۸ است، نه یک بیت پنجم.
- **Synchronous counter (چهار بیتی)**: چهار flip-flop JK که J,K هرکدام به هم وصل (T flip-flop)؛ کلاک همه به هم وصل؛ یک سیگنال count enable؛ T هر flip-flop = AND همه بیت‌های قبلی (که با ANDهای دو‌ورودی زنجیره‌ای ساخته می‌شود، نه یک AND بزرگ).

---

## ⚠️ تأکیدها و نکات امتحانی (Emphasis and exam hints)

- در shift register همه انتقال‌ها **همزمان** رخ می‌دهند (چون کلاک همه با هم می‌رسد)؛ اگر کلاک یکی دیرتر برسد (clock skew) ممکن است یک بیت دو بار جلو برود که نامطلوب است.
- در clock gating، ایراد اصلی **clock skew** است: کلاک gate شده کمی دیر می‌رسد و ممکن است اطلاعات درست load نشود. توضیح کامل «در درس‌های بعدی».
- «حفظ اطلاعات» یعنی **مستقل از ورودی‌ها** اطلاعات بماند؛ روش ثابت‌نگه‌داشتن ورودی «معقول نیست».
- **rotate** فقط با برگرداندن serial output به serial input به‌دست می‌آید؛ «روتیت همان شیفت است» با بازخورد بیت آخر.
- در serial adder، سؤال دانشجو درباره race بین کلاک shift registerها و کلاک D flip-flop (carry) مطرح شد. پاسخ: چون کلاک D flip-flop از یک AND gate عبور می‌کند (تأخیر یک gate) در حالی که خروجی full adder باید از چند gate عبور کند تا carry جدید بسازد، تأخیر مسیر full adder بیشتر است، پس carry واقعاً از قبل داخل flip-flop نوشته می‌شود و مشکل ایجاد نمی‌شود. ولی «اگر تأخیر full adder کمتر بود ممکن بود مشکل ایجاد شود». نکته کلیدی این است که مسیر carry در full adder (که دست‌کم از یک XOR و یک AND عبور می‌کند) تأخیر بیشتری از یک AND gate تنها دارد.
- تفاوت کلیدی ripple counter و synchronous counter: در ripple تغییرات **پله‌ای و با تأخیر** ripple می‌شوند؛ در synchronous همه flip-flopها **همزمان** تغییر می‌کنند و کلاک همه به هم وصل است.
- در ripple counter، لبه بالارونده یا پایین‌رونده تعیین می‌کند up یا down بشمارد؛ در synchronous counter لبه فرقی نمی‌کند چون کلاک‌ها مشترک‌اند.
- **مبنای شانزده در برابر پیمانه شانزده**: modulo 16 یعنی 0 تا 15 می‌شمارد و دوباره صفر می‌شود؛ این با base 16 (شمارش A,B,C,D,E,F) فرق دارد. transcript صریحاً این اشتباه رایج را رد می‌کند.
- در BCD counter، تشخیص عدد ده از روی بیت‌هایی که در ده یک هستند (Q4 و Q2) با NAND انجام می‌شود و reset را فعال می‌کند.
- synchronous counter «تمرین دارید»؛ جزئیات کامل جلسه بعد؛ کلیت آن با count enable و AND کردن بیت‌های قبلی برای T هر flip-flop.

---

## 🔗 ارتباط با جلسات دیگر (Connections)

- **جلسه قبل / یکی مانده به آخر**: register و مفهوم آن تعریف شده بود؛ این جلسه با state table و پیاده‌سازی register با D flip-flop ادامه می‌یابد و صریحاً به «همان روش جلسه قبل» برای طراحی مدار ترتیبی ارجاع می‌دهد.
- **مبحث combinational (جلسات قبل)**: full adder و adder چهار بیتی (بستن چهار full adder پشت سر هم) که مبنای serial adder این جلسه است. مسئله area بودن adder موازی انگیزه serial adder شد.
- **flip-flopها (D, JK, T)**: تعریف قبلی T flip-flop و ساخت T از JK (اتصال J و K) یادآوری شد و ساخت T از D در این جلسه حل شد.
- **جلسه بعد**: ادامه synchronous counter؛ transcript می‌گوید synchronous counter را کامل جلسه بعد توضیح می‌دهد.

---

## 📢 اطلاعات و اعلانات (Announcements)

- فقط یک جلسه دیگر (به‌جز این جلسه) از کلاس باقی مانده است.
- **یک‌شنبه** ادامه این مطالب گفته می‌شود.
- **سه‌شنبه** امتحان است.
- یک **کلاس رفع اشکال** قبل از امتحان برگزار می‌شود؛ در سایت درباره روز آن نظر بدهید. بیشترین تعداد نظردهنده‌ها **دوشنبه** (یک روز قبل از امتحان) را پیشنهاد داده‌اند، ولی تعداد نظردهنده‌ها کم بوده: از حدود ۲۰۰ دانشجو فقط حدود ۳۴ نفر نظر داده‌اند.
- فیلم‌ها روی سایت گذاشته می‌شود (طبق transcript به‌جز یک مورد کوتاه‌تر از نیم ساعت مربوط به پایان جلسه قبل).

---

## 📝 خلاصه (TL;DR)

این جلسه register و shift register را کامل می‌کند: register با parallel load به‌کمک MUX در ورودی هر D flip-flop، سه روش hold (نگه‌داشتن load، ثابت‌نگه‌داشتن ورودی که «معقول نیست»، و clock gating با ایراد clock skew). shift register (SISO/SIPO/PISO/PIPO)، unidirectional و bidirectional، و rotate با برگرداندن SO به SI. سپس serial adder با full adder و یک D flip-flop برای carry (و مدل sequential آن با یک state flip-flop برای carry، به‌همراه بحث race که به‌دلیل تأخیر بیشتر full adder مشکلی ایجاد نمی‌کند). universal shift register چهار بیتی با چهار عملکرد no change / shift right / shift left / parallel load که با S1 S0 و MUXها کنترل می‌شود. در بخش counter: T flip-flop از D (D = Q ⊕ T)، سپس four bit binary ripple counter با T flip-flop (نام‌های چهار stage، modulo 16، divide by 16)، up در برابر down counter با تغییر لبه یا استفاده از Q'، BCD ripple counter (شمارش 0 تا 9 با NAND و reset) و three decade BCD counter تا 999. در پایان مقدمه‌ای بر synchronous counter با count enable که کلاک همه flip-flopها مشترک است و T هر flip-flop AND بیت‌های قبلی است.