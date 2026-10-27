"يا هندسة، جالي سؤال في انترفيو عن Task.WhenAll وTask.WhenAny وTask.WaitAll، ودلوقتي كمان Task.WhenEach، وحسيت إن المعلومات عندي متلخبطة شوية، ممكن توضح الفرق بينهم؟"

بداية خلينا متفقين ان كلهم بيتعاملوا مع مجموعة من ال Tasks والفرق اللي بينهم امتى بدك النتائج وكيف بدك تنتظرها؟

• ال ()Task.WhenAll
تخيل صفحة Product في E-commerce محتاجة
- السعر من Pricing Service
- الكمية المتوفرة من Inventory Service
- التقييم من Reviews Service.

الثلاثة service calls  عبارة عن Independent calls يعني مستقلين لكن الصفحة محتاجة الثلاثة كلهم. فبتستخدم await Task.WhenAll علشان تبدأهم مع بعض واستنى الكل بشكل Asynchronus يعني بدون blocking ل thread

• ال ()Task.WhenAny
تخيل عندك تطبيق بده سعر صرف USD/EUR وعندك اكثر من Exchange Rate Provider اللي هم اللي بيزودوك بسعر الصرف وبدك تتعامل مع اول provider يرد

فبتستخدم await Task.WhenAny علشان بتبدأهم كلهم مع بعض واستنى اول Task تخلص بشكل asynchronous.  لكن مهم تعرف ان WhenAny ما بتلغي باقي ال Tasks  بشكل اوتوماتكي.

• ال ()Task.WaitAll
تخيل عندك synchronous Batch Application بعمل End-of-Day processing
 وبدأ 3 Tasks اللي همه 
- توليد Sales Report
- تجهيز Accounting Export
- عمل Backup.

الان حسب ال Requirement البرنامج ممنوع ينهي الـ processing قبل ما الثلاثة يخلصوا. فبتستخدمTask.WaitAll يعني: استنى الكل لكن الانتظار هون synchronous blocking بمعنى ان ال calling thread نفسه بضل واقف ومستني ووارد ان كل عملية من العمليات التلاثة تكون async بس Calling thread ل Task.WaitAll هو اللي بيكون عامل block ل Thread.

كمان في NET 9 طلعت كمان method هي Task.WhenEach
تخيل Dashboard بتجيب 20  Insight منserver. وانتى ما بدك تستنى الـ 20 كلهم وكمان ما بدك بس اول واحد.
انت بدك كل ما Server يرد تعرض نتيجته مباشرة ف هون Task.WhenEach بتخليك تتعامل مع كل Task حسب ترتيب اكتمالها.

بشكل مختصر..

• في WhenAll  ........  بدي الكل مع بعض "asynchronous wait"
• فيWhenAny ........ بدي اول واحدة تخلص "asynchronous wait"
• في WhenEach ........ بدي الكل بس اول بأول"asynchronous iteration" 
• في WaitAll ........ بدي الكل  "synchronous wait"

وبس خلاص!
