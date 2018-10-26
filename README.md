# ExamForiOS

## 基礎題
### Objective-C
- Block
 >* Block是Cocoa/CocoaTouchFramework中的匿名函式 (Anonymous Functions)的實作。
所謂的匿名函式，就是一段具有物件性質的程式碼，這一段程式碼可以當做函式執行，
另一方面，又可以當做物件傳遞;因為可以當做物件傳遞，
所以可以讓某段程式碼變成是某個物件的某個 property，或是當做 method 或是 function 的參數傳遞，
就是因為這種特性，造成最常使用 block 的時機，就是拿 block 實作 callback。
>* block 的好處比起傳統 delegate 的做法，他能夠將程式碼集中，讓程式架 構變得更簡單，可讀性更高。

- Multithreading(GCD)
 >* GrandCentralDispatch簡稱(GCD)是蘋果公司開發的技術，以優化 的應用程序支持多核心處理器和其他的對稱多處理系統的系統。這建立在任 務並行執行的線程池模式的基礎上的。

- Memory Leak&Retain Cycle (Memory Manaegment)
>* 你借了記憶體來用卻沒還回去，久了可能就會造成”漏水”(memory leaking)的情況。

- Delegate&Protocol
> * Delegate:中文翻譯成「委任」，委任兩字講的好聽是拜託別人做事，講 白一點就是自己不想做或不會做，所以外包出去叫別人做。但是，就算是要 叫別人做也不能隨便找一個路人就可以，舉個例子，我想要把「撰寫 Ruby 程式」這件事委任給別人，要有能力處理這份工作的人至少得知道 Ruby 程式怎麼寫。那要判斷對方是否有「能力」來接受我的委任，就是問這個被 委任的人是否有符合(Conform)我訂的條件(Protocol)，然後這個條件就跟 面試新人一樣，某些技能是必須的(Required)，但其它條件是非必須的 (Optional)。<br>
>* Protocol:協定，唯一個在類別間分享方法的清單，協定未列出的方法並無 相對應的實作方法，取決於使用者之後如何實作他。其中有一些是選擇性， 而其他是必要性的，若要實作必要性方法，則要遵從會採納該協定。

- Properties(strong/weak/etc.) 含义:
>* Copy:复制内容(深复制)，如果调用copy的是数组，则为指针复制 (浅复制)，仅仅复制子元素的指针。<br>
@property (nonatomic,copy)NSString *title;<br>
@property (nonatomic, copy) NSMutableArray *myArray;//not recommended <br>@property (nonatomic, copy) SomeBlockType someBlock;<br><br>
>*  Assign:对基础数据类型(NSInteger，CGFloat)和 C 数据类型(int, float, double, char 等)
@property (nonatomic, assign) int n;<br>
@property (nonatomic, assign) BOOL isOK;<br>
@property (nonatomic, assign) CGFloat scalarFloat;<br>
@property (nonatomic, assign) CGPoint scalarStruct;<br><br>
>* Strong:相当于retain。Strong在ARC环境为默认属性类型。<br>
@property (nonatomic,readwrite,strong)NSString *title; <br>
@property (strong, nonatomic) UIViewController *viewController; <br>
@property (nonatomic, strong) id childObject;<br><br>
>* Retain:NSObject 及其子类。Release 旧值，retain 新值。Retain 是指 针复制(浅复制)，引用计数加 1，而不会导致内容被复制。<br>
@property (nonatomic, retain)UIColor *myColor;<br><br>
>* Weak:取代之前的 assign，对象销毁之后会自动置为 nil，防止野指针。 Assign 不能自动置为 nil，需要手动置为 nil。Delegate 基本总是使用 weak，以防止循环引用。特殊情况是，希望在 dealloc 中调用 delegate 的某些方法进行释放，此时如果使用 weak 将引起异常，因为此时已经是 nil 了，那么采用 assign 更为合适。<br>
@property (weak, nonatomic) IBOutlet UIButton *myButton;//处于最顶层的 IBOutlet 应该为 strong<br>
@property (nonatomic, weak) id parentObject; <br>
@property(nonatomic,readwrite,weak) id <MyDelegate> delegate; <br>
@property (nonatomic, weak) NSObject <SomeDelegate> *delegate; <br><br>
>* Readonly:此标记说明属性是只读的，默认的标记是读写，如果你指定了 只读，在@implementation 中只需要一个读取器。或者如果你使用 @synthesize 关键字，也是有读取器方法被解析。而且如果你试图使用点操 作符为属性赋值，你将得到一个编译错误。<br><br>
>* Readwrite:此标记说明属性会被当成读写的，这也是默认属性。设置器和 读取器都需要在@implementation 中实现。如果使用@synthesize 关键 字，读取器和设置器都会被解析。<br><br>
>* unsafe_unretained:unretained且unsafe，由于是unretained所以与 weak 有点类似，但是它是 unsafe 的.
@property(nonatomic,unsafe_unretained)Book *book1;<br>
unsafe_unretained id safeSelf = self;<br>

- @property
>* @Property 是声明属性的语法，它可以快速方便的为 实例变量创建存取器，并允许我们通过点语法使用存取器。存取器 (accessor):指用于获取和设置实例变量的方法。用于获取实例变量值的 存取器是 getter，用于设置实例变量值的存取器是 setter。

- Instruments:
>* Xcode內建的檢測儀器

- Category:
>* 不用繼承物件，就直接增加新的method，或替換原本的method。

- NSOperationQueue 和 GCD 的差別
>* GCD是底层的C语言构成的API，而NSOperationQueue及相关对象是 Objc 的对象。在 GCD 中，在队列中执行的是由 block 构成的任务，这是 一个轻量级的数据结构;而 Operation 作为一个对象，为我们提供了更多 的选择;
>* 在 NSOperationQueue 中，我们可以随时取消已经设定要准备执行的任务 (当然，已经开始的任务就无法阻止了)，而 GCD 没法停止已经加入 queue 的 block(其实是有的，但需要许多复杂的代码);
>* NSOperation能够方便地设置依赖关系，我们可以让一个Operation依赖 于另一个 Operation，这样的话尽管两个 Operation 处于同一个并行队列 中，但前者会直到后者执行完毕后再执行;
>* 我们能将KVO应用在NSOperation中，可以监听一个Operation是否完 成或取消，这样子能比 GCD 更加有效地掌控我们执行的后台任务;
>* 在 NSOperation 中，我们能够设置 NSOperation 的 priority 优先级，能够 使同一个并行队列中的任务区分先后地执行，而在 GCD 中，我们只能区分 不同任务队列的优先级，如果要区分 block 任务的优先级，也需要大量的复 杂代码;
>* 我们能够对 NSOperation 进行继承，在这之上添加成员变量与成员方法， 提高整个代码的复用度，这比简单地将 block 任务排入执行队列更有自由 度，能够在其之上添加更多自定制的功能。
- UIKit 一定會問，常見的就是 View Hierarchy 與 UIControl 家族，深入點的有 Layer 和 The Responder Chain

- 共享數據:<br>
>* URLScheme<br>
平常我們會去呼叫其它的 App 來達到我們的⺫的，如想要開啟網頁就 會叫出 Safari App，是怎麼做到的呢?就是使用 URL Scheme，格 式:<br>
  schemename://<br>
  schemename 可以是以下幾個例子:<br>
  http, https, ftp : Web links ((launches the Safari app)<br>
  mailto :E-maillinks(launchestheMailapp)<br>
  tel : Telephone Numbers (launches the Phone app)<br>
  sms : Text Messages (launches the SMS app) 那如果我們想要讓他人能夠開啟我們的 App，又該如何做到呢?就來 客製化 URL Scheme (Custom URL Scheme)吧!<br>
>* KeyChain<br>
Keychain 是 iOS 所提供的一個安全儲存參數的方式，最常用來當作儲 存帳號、密碼、信用卡資料等需要保密的資訊，Keychain 會以加密的 方式將這些資訊儲存於裝置當中。
由於 Keychain 的資料並不是儲存在 App 的 Sandbox 中，所以即使 將 App 從裝置中刪除了，這些資料還是存在於裝置中，當使用者重新 安裝了相同的 App 後，這些資訊還是可以被取得。 另一個特色是，Keychain 的資料可以透過 Group Access 的方式，讓 資料可以在 App 間共享，Google 系列的 App (Gmail、Google+、 日曆...)就是透過這樣的方式來紀錄使用者登入資訊，只要使用者在其 中一個 App 中完成登入了，其他的 App 也可以讀取到同相的登入資 訊進行登入。
>* 剪貼簿<br>
- JavaScript與ObjC的溝通
>* UIWebViewDelegate
>* JavaScriptCore
- CI，UnitTest，AutoTest也有不少公司問到 
- 生命週期(ViewController&Application)，
>* viewController <br>
init<br>
loadView<br>
viewDidLoad <br>
viewWillAppear <br>
viewDidAppear <br>
viewWillDisappear <br>
viewDidDisappear <br>
viewWillUnload <br>
viewDidUnload <br>
dealloc<br><br>
>* Application:<br>
 i. application:willFinishLaunchingWithOptions: - 这个方法是你在启动时的第一次机会来执行代码<br>
ii. application:didFinishLaunchingWithOptions: - 这个方法允许你在显示 app 给用户之前执行最后的初始化操作<br>
iii. applicationDidBecomeActive: - app 已经切换到 active 状态后需要执行的操作<br>
iv. applicationWillResignActive: - app 将要从前台切换到后台时需要执行的操作<br>
v. applicationDidEnterBackground: - app 已经进入后台后需要执行的操作<br>
vi. applicationWillEnterForeground: - app 将要从后台切换到前台需要执行的操作，但 app 还不是 active 状态<br>
vii. applicationWillTerminate: - app 将要结束时需要执行的操作<br>
- KVO觀念:
>* KVO是Object-C中定義的一個通知機制，其定義了一種對象間監控對方狀態的改變，並做出反應的機制。對象可以為自己的屬性註冊觀察者，當這個屬性的值發生了改變，系統會對這些註冊的觀察者做出通知。
- Singleton觀念:
>* 有些時候為了不重覆去做某樣事情而降低了效率，例如:打開資料庫的連接，如果每次用的時候打開，用完立即關閉，下次再用時又要打開連接，這樣的效率不會高，這時候可以使用 Design Pattern 裡的 Singleton 模式去 減少資料庫的連接次數。<br>
Singleton 模式確保了不同的類別也使用相同的實例，並不會同時擁有 2 個或以上的實例，這種模式是非常常用的，例如 Java EE 的 Spring Framework 預設創造的 Spring Bean 也是使用 Singleton 模式的，但 Singleton 模式亦有一個壞處，就是應用程式使用了 Thread 時，同時有 2 個類別也更改 Singleton 的類別內容可能會出問題，你需要在 Objective-C 內使用 @synchronized 來令到程式不能同時更改某些內容，強迫一個執行完成完才到下一個，但使用這方法效率也會下降的。
- ARC的機制是什麼?、MRC
- @proprety屬性、Strong&Weak對記憶體的影響 
- Json、XML
- closures 是什麼?
- Classmethod & instancemethod
>* instancemethod(宣告時在開頭用-)
>* class method (宣告時在開頭用 + )用法像是 JAVA 的 static method 差別在於 instance method 要先實例化, class method 可以直接透過 Object 使用
- Push 3 viewController and how to get variable in fristviewcontroller
>* 使用singleton
- Two Uitableview in same viewcontroller
>* 跟一般的tableview做法一樣，只是多一個判斷tableview== self.firstTableView
- Testcode
- Provisioning & Appid & codesign & certificate
>* AppID即ProductID,用於標識一個或者一組App。
>* Certificate:證書是一個經證書授權中心數字簽名的包含公開密鑰擁有者信
息以及公開密鑰的文件。最簡單的證書包含一個公開密鑰、名稱以及證書授 權中心的數字簽名。 數字證書還有一個重要的特徵就是時效性:只在特定的時間段內有效。
>* Codesign:Codesigning对你来说，最主要的意义就是它能让你的App在设备上运行。不管是你自己的设备，甲方客户的，还是在 App store 上购买你的消费者。
>* ProvisioningProfiles(提供描述檔):<br>
i. 哪個 App ID (Bundle Identifier)<br>
ii. 使用哪張 Certificates (憑證)<br>
iii. 在哪些 Devices (裝置)可使用<br>
- AFNetworking Synchronize
>* NSOperation的operation wait Until Finished
>* Use Block - typedef void (^completion_handler_t)(User*, NSError*); 
>* 第三方套件-AFNetworking-Synchronous
- XcodeSnippets


## 進階題
- MVC是什麼,優缺點?
>* 優點:<br>
MVC 指的是 Model-View-Controller 架構，就是要在系統架構中，將資 料模組(Model)、呈現模組(View)、控制模組(Controller)三者分離，這樣的做法可以降低系統內的功能邏輯和資料的耦合，使得架構上分工明確、促使開發人員專責分工，進而提昇系統的擴充性。<br>
>* 缺點: <br>
没有明确的定义、不适合小型，中等规模的应用程序，增加系统结构和实现的复杂性
