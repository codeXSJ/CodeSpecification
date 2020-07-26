# 项目代码规范

### 命名规范
我们尽可能遵守 Apple 的命名约定, 其推荐使用长的,描述性强的方法和变量名,使其阅读起来更加清晰易懂。不能随意使用缩写,导致其他人员阅读代码困难。 [cocoa 编码指南](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingBasics.html#//apple_ref/doc/uid/20001281-1001751-BBCFECGB)
***
### 驼峰命名
针对属性,变量,方法等均采用 `小写字母` 开头的驼峰命名准则。
***
### 前缀
项目名称,类名,文件名都应该保持一致的前缀名,根据 Apple Guide 建议类名前缀应该使用 2 个英文以上最好,因为 Apple 写的框架都是直接使用 2 个英文字母开头, 使用 3 个字母 能有效防止类名重复影响工程。

我们使用 `XXX(公司名的缩写)` 前缀
***
### 方法命名
根据 Cocoa 命名方法规则,我们应该准守这几个点。

使用小写字母开头,后面嵌套连接的字母使用大写开头。不过在写Category Method的时候, 也尽量加入**XXX**前缀，但不强求；

对于采取动作行为的方法,使用动词开头,但是不要直接使用 **do** 或者 **does** 。

每个方法参数前必须带有相同或者能清晰表达其原意的关键字。

子类化创建相对父类更加详细功能的方法时应该把新增参数添加在原有方法的后面。

假如方法名过长的时候可以采用每个参数独占一行的规则,并保持每个参数分号 : 对齐的方式排列。

实例方法和类方法 (-/+) 符号后面应该保持一个**空格**, 如:
```
- (void)starMonitoring;
```
***
### 命名属性和实例变量
一般属性一致采用 `@property` 进行声明, 特殊的数据类型可以使用实例变量声明。

`@property (nonatomic, copy) NSString *name;`
属性关键字首个应该是 原子性,到内存管理关键词。如果需要写读写关键字的话, 其排在第二位.如: `@property (nonatomic, readwrite, copy) NSString *name;`

声明实例变量时必须采用 `_` 下划线作为变量名前缀。

添加 `@property` 相应代码段, 提高编码速度。
***
### 命名头文件
* 头文件名必须采用一致的前缀开头, 加上其代表相关类名代表的形容词即可。尽量不要使用缩写,宁愿名字稍微长点儿,也要让其有一目了然的效果。
* 声明相关的类扩展和协议时,必须将其声明放在主类文件夹里面,这样阅读起来相对方便。
* 如果有多个功能相似的类,可以考虑将其划分为一个框架,使用 .h 文件进行声明管理。
***
### 命名 Category 方法
在写添加类别方法时, 必须采用`前缀名_ 后连接对应方法名`的方式来进行添加。这样就有效的避免,覆盖掉系统原有或新增方法, 导致意想不到的问题发生。
如: `- (void)XXX_methodName`
***
### 统一的ViewController 模板
针对大多数没有特别复杂逻辑的控制器实现文件, 我们制定了一套 @implementation 实现模板.

如下：
```
#import "XXXHomeViewController.h"
    
@interface XXXHomeViewController ()
    
@end
    
@implementation XXXHomeViewController
    
#pragma mark - 1.Life Cycle
    
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.
    // 1.设置导航栏
    [self setupNavBar];
    // 2.设置view
    [self setupView];
    // 3.请求数据
    [self setupData];
    // 4.设置通知
    [self setupNotification];
}
    
#pragma mark - 2.Setting View and Style
    
- (void)setupNavBar {
     
}
    
- (void)setupView {
    
}
    
- (void)setupData {
   
}

#pragma mark - Action 相关事件方法
    
#pragma mark - lazy load
    
#pragma mark - UITableView 相关 delegate datasource

#pragma mark - UICollectionView

#pragma mark - 其他一些自定义view代理

#pragma mark - Networking 网络请求
```
可以到 Xcode 修改 ViewController 文件模板, 创建的时候即自动生成模板样式。
***
### 点语法
在访问属性或者修改的过程中,应该全程使用 . 语法来进行。 能提高阅读性同时, 还能减少代码长度。但是又不能滥用。

推荐：
```
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```
反对：
```
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```
***
### 私有属性
要求私有属性必须在类 extension 下使用 @peoperty 声明.

例如:
```
@interface XXXHomeController ()
    
@property (nonatomic, strong) NSDate *lastRefreshTokenTime;
@property (nonatomic, assign) CCRefreshTokenStatus lastRefreshTokenStatus;

//编码后的App版本号，6位数字，如3.0.1为030001
@property (nonatomic, copy) NSString *encodeVersion;     
@end
```
***
### 常量
#### 枚举常量
要求使用 `NS_ENUM()` 函数而非 ~~enum()~~ 函数进行声明。 其具有更强的类型检查和代码补全功能。
例如代表网络状态的常量, 网络连接成功, 失败,正在连接等。 我们通常采用枚举的方式表示。

例如:
```
typedef NS_ENUM(NSUInteger, MyEnum) {
    MyEnumValueA = 0,
    MyEnumValueB,
    MyEnumValueC,
};
```
### 其他类型常量
**多用类型常量, 少用 #define 预处理指令.**

>不要使用#define预处理指令来定义常量,因为这样的常量不包括类型信息,编译器只会在编译前执行查找和替换操作,即使有人重新定义了常量值,编译器也不会发出警告,这样会导致应用程序中的常量值不一致.

>在实现文件.m中使用static const来定义"只在编译单元张可见的常量",由于此类常量不在全局符号表中,所以无须为其名称加前缀.

>在头文件中使用extern来声明全局常量,并在相关文件中定义其值,这种常量要出现在全局符号表中,所以其名称应该加以区分,通常用与之相关的类名做前缀.
[参考链接](https://www.jianshu.com/p/8d53a447fb85)
***
### 文件引入方式
在 .h 文件中尽量使用 @class 声明文件, 直到 .m 文件中真正需要的时候在使用 @improt 进行引用, 能有效的防止其它类引入此类时, 一并也引入了

@improt 文件顺序: 可以先写引入系统文件, 依次到 Public.h 最后才到我们自己编写的文件。

检查文件,避免引入到没有使用到的文件,发现应及时清除。
***
### 多用字面量语法,少用与之等价的语法
我们在创建 NS 开头的数据类型的时候, 一律采用字面量语法的形式进行创建。

推荐:
```
NSArray *array = @[@"123", @"456"];
NSDictionary *dictionary = @[@"name": @"jersey", @"age": @"61"];
NSNumber　*number = @18;
NSString　*string = array[0];
```
反对:
```
NSArray *array = [NSArray arrayWithObjects:@"123", @"456", nil];
[NSDictionary dictionaryWithObjectsAndKeys:@"jersey", @"name", @"61", @"age", nil];
NSNumber* number = [NSNumber numberWithInt: 18];
NSString　*string = [array objectAtIndex: 1];
```
**功能一致,但是使用字面量能令代码更为整洁。并且对于创建数组,字典使用字面量语法创建,当误传 nil 时能更早的发现错误。**
***
### 间距
* 不要使用 tab 键来进行缩进, 应该使用 4 个空格来代替。

* 方法的大括号和其他的大括号（if/else/switch/while 等等）始终和声明在同一行开始，在新的一行结束。

* 方法之间应该正好空一行，这有助于视觉清晰度和代码组织性。在方法中的功能块之间应该使用空白分开，但往往可能应该创建一个新的方法。

推荐:
```
if (user.isHappy) {
    // Do something
} else {
    // Do something else
}
```
***
### 条件语句
* 条件判断语句执行主体不管是 if 里面还是 else 里面都必须要使用大括号包住, 即使代码是一行的情况（Apple SSL/TLS gotofail）。 以及 for 循环。
* if-else中，else不另起一行，与if的反括号在同一行，如上段代码所示，注意else前后均有空格。
* switch-case中，case后的代码多余一行，则需要{}包裹，建议所有case和default后的代码块均用{}包裹。
**涉及位置：@interface、方法实现、if-else、switch-case等。
优点： 减少代码冗余行数，代码更加紧凑，结构清晰。**

推荐:
```
if（hasSillyName）{
    LaughOutLoud（）
} else {
    BlowTheHorn（）;
}

for（int i = 0 ; i < 10 ; i ++）{
    BlowTheHorn（）;
}
```
反对:
```
if（hasSillyName）
    LaughOutLoud（）;               //避免。

for（int i = 0 ; i < 10 ; i ++）
    BlowTheHorn（）;                //避免。
```
***
### 避免尤达表达式
不要使用尤达表达式。尤达表达式是指，拿一个常量去和变量比较而不是拿变量去和常量比较。它就像是在表达 “蓝色是不是天空的颜色” 或者 “高个是不是这个男人的属性” 而不是 “天空是不是蓝的” 或者 “这个男人是不是高个子的”

表达式条件应该是先写变量在到常量的判断过程

推荐:
```
if ([myValue isEqual:@42]) { 

} 
```
反对:
```
if ([@42 isEqual:myValue]) {

}
```
***
### 黄金大道
在使用条件语句编程时,尽管遇到逻辑复杂的代码,我们也应该尽量避免其嵌套导致阅读困难。
尽量使用 return 将不符合逻辑的直接忽略掉. 然后将要执行的代码放到判断语句外面,减少嵌套。

推荐:
```
- (void)someMethod {
    if (![someOther boolValue]) {
        return;
    }
    // Do something important
}
```
反对:
```
- (void)someMethod {
    if ([someOther boolValue]) {
        // Do something important
    }
}
```
***
### 复杂的表达式
当出现复杂的 if 条件的时候, 可以把它们分别提取出来赋值到 BOOL 变量上, 让逻辑更加清晰明了, 约定 2 个条件以上则需要将其单独提取出来。

例如:
```
BOOL nameContainsSwift  = [sessionName containsString:@"Swift"];
BOOL isCurrentYear      = [sessionDateCompontents year] == 2014;
BOOL isSwiftSession     = nameContainsSwift && isCurrentYear;

if (isSwiftSession) {
    // Do something very cool
}
```
***
### 三元运算符
推荐使用三元运算符进行运算. 它能使代码更加简洁清晰。

推荐:
```
result = a > b ? x : y;
```
不推荐:
```
result = a > b ? x = c > d ? c : d : y;
```
当三元运算符的第二个参数（if 分支）返回和条件语句中已经检查的对象一样的对象的时候，下面的表达方式更灵巧：

推荐:
```
result = object ? : [self createObject];
```
不推荐:
```
result = object ? object : [self createObject];
```
***
### 错误处理
很多系统方法通过 error 返回指针的形式来表示错误, 我们应该针对其返回值判断, 而非错误变量。

推荐:
```
NSError *error;
if (![self trySomethingWithError:&error]) {
    // 处理错误
}
```
反对:
```
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // 处理错误
}
```
一些苹果的 API 在成功的情况下会写一些垃圾值给错误参数（如果非空），所以针对错误变量可能会造成虚假结果（以及接下来的崩溃）。
***
### NSNotification
约定在我们自己定义 NSNotification 的时候应该把通知的名字定义为一个字符串常量, 就像把我们暴露给其他类的字符串常量一样。使用 extern 关键字将其在 .h 文件声明, 并且在 .m 文件对其定义。

**使用约定的 XXX（公司名缩写） 作为前缀, 后跟具体通知名称, 同时推荐使用 Did/Will 这样的动词连接表示最好**

推荐:
```
XXXBadgeManager.h
extern NSString *const XXXBadgeInfoDidUpdateNotification;

XXXBadgeManager.m
NSString *const XXXBadgeInfoDidUpdateNotification = @"XXXBadgeInfoDidUpdateNotification";
```
***
### Self 循环引用
我们在写 block 回调的时候, 特别是对于实例方法中调用, 很容易导致其循环引用。
我们在项目中需要引入一组强弱引用的宏定义. @weakify(self) 、@strongify(self)
我们在遇到需要修饰 Self 的时候直接使用这组宏来进行修饰即可。。

例如:
```
[JSDStartInfo getStartInfoSuccess:^{
   @weakify(self)
   [JSDCheckVersion checkWithPass:^{
       @strongify(self);
       [self reloadView];
   }];
}];
```
***
### MVC模式
项目文件结构主要以项目每个大模块相应下来进行划分。如 Main,Home,Invest,Discover,Wealth,More,Account。 这几个主要功能模块进行一一划分、
我们想新建添加模块的时候, 必须遵守这个原则去创建文件夹。

code 一致采用 MVC 设计框架, 每个模块功能点一般划分成 Model,ViewController,View 去进行实现. 每个模块各执其职。

* Model: 保存相应的数据结构, 逻辑复杂的情况下, 可以将数据操作逻辑从 ViewController 移动到 Model 去进行实现, 防止 ViewController 代码过于冗余. 不方便阅读。
* ViewController: 主要负责获取 Model 层数据, 进而管理 View 层. 以及 User 一系列交互逻辑, 都由其负责响应管理。 由于此层任务繁重, 逻辑复杂的时候容易造成代码冗余的状态, 必要的时候我们也可以更换成 MVVM 模式去有效的避免 MVC 这个缺点。
* View: 主要负责接收 Model 层数据, 进行页面渲染 数据展示, 一般无特殊情况, 建议使用代码去完成界面构建。
***
### 图片管理
项目中的图片一致存放到 Assets.xcassets 进行管理. 

**图片名称: 图片名称可以以模块作为首接 _ 连接其相应功能点。这样在以后查找起来也相对方便**
如: icon_trade_result_fail 投资,结果页,状态。 icon_discover_activity 发现,活动
***
### Xcode 版本
Xcode 版本更新迭代很快, 建议定时到 App Store 进行更新到最新版。
***
### 三方库管理工具
使用 Cocoapods 统一对第三库进行管理, 建议定时更新到最新版本, 保持版本一致。
***
### TODO
当在开发某个功能中遇到一些疑难杂症的问题, 但是又由于时间紧急, 来不及把细节做到完美, 如果在不影响当前功能使用的情况下, 为了防止在后面的开发中遗忘掉, 我们必须要在相应的地方添加注释 然后以 **TODO:** 的格式, 接相关问题描述添加进来, 待后面有空闲时间之后回来进行修复完善。
***
### CGRect 使用（忽略）
当需要访问 CGRect 中某个成员时, 应该使用 CGGeometry 函数来直接访问。 而不是使用 .语法来获取。

推荐:
```
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```
反对:
```
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```
***
### 防止重复造轮子
在着手开发新需求之前,应先观察一下项目中,是否有已经封装好的相关功能类,如果已经有了,最好在当前类上添加 Catogory 进行扩展。

项目中的基础功能类, 以及对应封装的工具一致保存在 Util 。
***
### 单例
要求必须使用线程安全的 GCD 函数进行创建。

例如:
```
+ (instancetype)shareInstance {
    static id sharedInstance = nil;
    
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedInstance = [[self alloc] init];
    });
    return sharedInstance;
}
```
***
### 关于缩写
Objective-C 本身就是让阅读者读起来就像在阅读句子一样的一门语言, 变量, 常量 特别是方法名相对其他语言来说会相对长一点,主要用意就是为了表达更加清晰明了, 尽量不让阅读的人产生歧义,并且基本能达到光看代码名字就知道其用意。
所以我们也应该严格遵守语言的设计风格, 和大家保持一致, 能有效的提高我们协同开发的效率同时,也能使我们能编写出更容易让大家接受的代码。

主要参考文档: [Cocoa guide 可接受的缩略语和首字母缩略词](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/APIAbbreviations.html#//apple_ref/doc/uid/20001285-BCIHCGAE)

不推荐大家使用缩写,宁愿命名长一点,也一定要保证语义表达清晰的原则。
