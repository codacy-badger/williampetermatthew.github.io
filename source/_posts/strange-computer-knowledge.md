---
title: 一些奇奇怪怪的计算机知识
date: 2018-09-03 19:07:10	
tags: 
  - 不知道什么东西
---
# 时间复杂度符号
- Θ，读音：theta、西塔；既是上界也是下界(tight)，等于的意思。
- Ο，读音：big-oh、欧米可荣（大写）；表示上界(tightness unknown)，小于等于的意思。
- ο，读音：small-oh、欧米可荣（小写）；表示上界(not tight)，小于的意思。
- Ω，读音：big omega、欧米伽（大写）；表示下界(tightness unknown)，大于等于的意思。
- ω，读音：small omega、欧米伽（小写）；表示下界(not tight)，大于的意思。


Ο是渐进上界，Ω是渐进下界。Θ需同时满足大Ο和Ω，故称为确界（必须同时符合上界和下界）。Ο极其有用，因为它表示了最差性能。

# 数字
- 1226563rd prime is 19260803
- **1226564th prime is 19260817**
- 1226565th prime is 19260823
- 1240302nd prime is 19490963
- **1240303rd prime is 19491001**
- 1240304th prime is 19491007
- 19260802nd prime is 359003321
- 19260803rd prime is 359003327
- 19260804th prime is 359003401
- 19260816th prime is 359003639
- **19260817th prime is 359003647**
- 19260818th prime is 359003669
- 19260822nd prime is 359003737
- 19260823rd prime is 359003747
- 19260824th prime is 359003753
- 19490962nd prime is 363541751
- 19490963rd prime is 363541781
- 19490964th prime is 363541807
- 19491000th prime is 363542381
- **19491001st prime is 363542383**
- 19491002nd prime is 363542411
- 19491006th prime is 363542461
- 19491007th prime is 363542477
- 19491008th prime is 363542503

# 编译错误？
```
 fatal error: XXX: No such file or directory #include<XXX>
 ^ compilation terminated.
```
上述的编译错误是指编译器由于没有发现XXX这个库所以终止了编译，这时候需要检查编译终止的库文件名的拼写是否有问题。
```
 error: 'XXX' was not declared in this scope XXX(***);
 ^
```
上述的编译错误是指编译器没有发现你定义的XXX这个函数，这时候需要检查你定义的函数名和使用时的是否一致。
```
 error: expected ',' or ';' before 'XXX' XXX
 ^
```
上述的编译错误是指编译器在编译XXX和上一条语句是没有发现语句分隔符','和';'，你需要检查上一条语句并添加分隔符。
```
 error: 'XXX' was not declared in this scope ***
 ^
```
上述的编译错误是指编译器在编译```***```这条语句的时候没有发现其中的XXX代表什么含义，你需要检查这条语句是否写错或者XXX是否被正确地定义。
```
 Can't compile file: AAA.cpp: In function 'BBB':
 AAA.cpp:7:20: error: expected ':' before ';' token
 CCC?DDD;:EEE;
 ^
 AAA.cpp:7:20: error: expected primary-expression before ';' token
 AAA.cpp:7:21: error: expected primary-expression before ':' token
 CCC?DDD;:EEE;
 ^
```
这个错误是我这个大蒟蒻以前犯下的一个错误，这个错误是由于三目运算符的位置中不能有语句分隔符','和';'。
```
 error: expected primary-expression before 'XXX' scanf(***),XXX;
 ^
```
这个错误是因为scanf不能使用','分隔的关系。
```
 error: lvalue required as increment operand i=++j++;
 ^
```
这个错误真的是我当时不知道为啥脑子抽掉写的，我也不知道别的运算符使用错误是否会这样提示，但是++只能用在前或后不能同时用。
```
 error: expected unqualified-id before ',' token typename AAA,,BBB;
 ^
```
这个错误是因为定义类型的时候不能空一个。
```
 error：conflicting declaration ‘typename AAA[***]’ typename AAA[***]; ^ note：previous declaration as ‘typename AAA’ typename AAA;
```
这个错误的实质是AAA重复定义，然后会产生很多错误信息。
```
 Can't compile file: AAA.cpp:3:1: error: 'XXX' does not name a type
 XXX BBB;
 ^
```
这是由于类型定义错误产生的错误。
```
 error：name lookup of 'i' changed for ISO 'for' scoping [-fpermissive] i ^
 note：if you use ‘-fpermissive’ G++ will accept your code 
 warning：right operand of comma operator has no effect [-Wunused-value] i ^
```
这是由于之前定义的局部变量被重新使用导致的，其本质是未定义。

***注：由于洛谷第4代评测机已经推出了中文编译信息，因此上帖可能暂时停止更新***

# 缩写
```
cpp：c plus plus C++
io(或I/O)：Input/Output 输入输出
std：standard 标准
int：integer 整型（在C++中也可定义为long，其实质为long int）
shrt：short 短整型
```
# 数字
```
2147483648：2的31次方，其负数是int类型的数据最小值
2147483647：2的31次方减1，是int类型的数据最大值
536870911：int数组下标乘积之和最大值
33554432：128MB空间最大可开的int数
```
# 函数
```
int isspace(a)//用于判断字符a是否为分隔符，这包括' '(32)、'\t'(9)、'\n'(10)、'\v'(11)、'\f'(12)和'\r'(13)
int isprint(a)//用于判断字符a是否为可打印字符，这包括所有具有占位功能的字符
int iscntrl(a)//用于判断字符a是否为控制字符，这除去了上文所述的可打印字符
int isupper(a)//用于判断字符a是否为大写字母
int islower(a)//用于判断字符a是否为小写字母
int isalpha(a)//用于判断字符a是否为字母
int isdigit(a)//用于判断字符a是否为数字
int ispunct(a)//用于判断字符a是否为标点符号
int isxdigit(a)//用于判断字符a是否为十六进制数字，这包括'0'(48)-'9'(57)、'a'(97)-'f'(102)和'A'(65)-'F'(70)
int isalnum(a)//用于判断字符a是否为字母或数字
int isgragh(a)//用于判断字符a是否为图形表示的字符，这包括上文所说的可打印字符中除去分隔符的部分
/*C++11警告*/int blank(a)//用途同isspace
int toupper(a)//当字符a是小写字母时,改为大写,其余情况不变
int tolower(a)//当字符a是大写字母时,改为小写,其余情况不变
```
# 这一波优化我看着都怕
```cpp
#pragma GCC optimize(2)
#pragma GCC optimize(3)
#pragma GCC optimize("Ofast")
#pragma GCC optimize("inline")
#pragma GCC optimize("-fgcse")
#pragma GCC optimize("-fgcse-lm")
#pragma GCC optimize("-fipa-sra")
#pragma GCC optimize("-ftree-pre")
#pragma GCC optimize("-ftree-vrp")
#pragma GCC optimize("-fpeephole2")
#pragma GCC optimize("-ffast-math")
#pragma GCC optimize("-fsched-spec")
#pragma GCC optimize("unroll-loops")
#pragma GCC optimize("-falign-jumps")
#pragma GCC optimize("-falign-loops")
#pragma GCC optimize("-falign-labels")
#pragma GCC optimize("-fdevirtualize")
#pragma GCC optimize("-fcaller-saves")
#pragma GCC optimize("-fcrossjumping")
#pragma GCC optimize("-fthread-jumps")
#pragma GCC optimize("-funroll-loops")
#pragma GCC optimize("-fwhole-program")
#pragma GCC optimize("-freorder-blocks")
#pragma GCC optimize("-fschedule-insns")
#pragma GCC optimize("inline-functions")
#pragma GCC optimize("-ftree-tail-merge")
#pragma GCC optimize("-fschedule-insns2")
#pragma GCC optimize("-fstrict-aliasing")
#pragma GCC optimize("-fstrict-overflow")
#pragma GCC optimize("-falign-functions")
#pragma GCC optimize("-fcse-skip-blocks")
#pragma GCC optimize("-fcse-follow-jumps")
#pragma GCC optimize("-fsched-interblock")
#pragma GCC optimize("-fpartial-inlining")
#pragma GCC optimize("no-stack-protector")
#pragma GCC optimize("-freorder-functions")
#pragma GCC optimize("-findirect-inlining")
#pragma GCC optimize("-fhoist-adjacent-loads")
#pragma GCC optimize("-frerun-cse-after-loop")
#pragma GCC optimize("inline-small-functions")
#pragma GCC optimize("-finline-small-functions")
#pragma GCC optimize("-ftree-switch-conversion")
#pragma GCC optimize("-foptimize-sibling-calls")
#pragma GCC optimize("-fexpensive-optimizations")
#pragma GCC optimize("-funsafe-loop-optimizations")
#pragma GCC optimize("inline-functions-called-once")
#pragma GCC optimize("-fdelete-null-pointer-checks")
```

# 101条计算机经典名言（英汉对照版）
> “People always fear change. People feared electricity when it was invented, didn’t they? People feared coal, they feared gas-powered engines. There will always be ignorance, and ignorance leads to fear. But with time, people will come to accept their silicon masters.”  
> As Bill Gates once warned, computers have indeed become our silicon masters, pervading nearly every aspect of our modern lives. As a result, some of the greatest minds of our time have pondered the significance of computers and software on the human condition. Following are 101 great quotes about computers, with an emphasis on programming, since after all this is a software development site.  
> “人们总是害怕改变。电被发明出来的时候他们害怕电，是不是？他们害怕煤，害怕蒸汽机车。无知无所不在，并导致恐惧。但随着时间推移，人们终究会接受最新的科技。”  
> 正如比尔盖茨曾经警告过一样，计算机已经真正成为我们的最新科技，几乎遍布我们日常生活的每一方面。所以，我们这个时代的某些最伟大的头脑开始思索起计算机和软件对于人类的重要性来了。以下就是101条有关计算机的伟大名言，并且，既然我们这个网站是一个软件开发网站，我们尤其关注编程方面的。  
> ## Computers计算机  
> 1. “Computers are useless. They can only give you answers.”  
> (Pablo Picasso)  
> “计算机没什么用。他们只会告诉你答案。”  
> (巴勃罗·毕加索，画家)  
> 2. “Computers are like bikinis. They save people a lot of guesswork.”  
> (Sam Ewing)  
> “计算机就跟比基尼一样，省去了人们许多的胡思乱想。”  
> (萨姆·尤因，作家)  
> 3. “They have computers, and they may have other weapons of mass destruction.”  
> (Janet Reno)  
> “他们拥有计算机，他们也还可能拥有其他的大规模杀伤性武器。”  
> (珍内特·雷诺，美国前女司法部长)  
> 4. “That’s what’s cool about working with computers. They don’t argue, they remember everything, and they don’t drink all your beer.”  
> (Paul Leary)  
> “跟计算机工作酷就酷在这里，它们不会生气，能记住所有东西，还有，它们不会喝光你的啤酒。”  
> (保罗·利里，吉他手)  
> 5. “If the automobile had followed the same development cycle as the computer, a Rolls-Royce would today cost $100, get a million miles per gallon, and explode once a year, killing everyone inside.”  
> (Robert X. Cringely)  
> “如果汽车能赶上计算机的发展周期的话，一辆今天的劳斯莱斯仅值100美元，每加仑要跑100万英里，每年还得爆炸一次，把里面的人杀个精光。”  
> (Robert X. Cringely，技术作家)  
>   
> ## Computer Intelligence计算机智能
> 1. “Computers are getting smarter all the time. Scientists tell us that soon they will be able to talk to us. (And by ‘they’, I mean ‘computers’. I doubt scientists will ever be able to talk to us.)”  
> (Dave Barry)  
> “计算机总是越来越智能的。科学家告诉我们说不久它们就能跟我们对话了。（这里的“它们”，我指的是“计算机”。我怀疑科学家永远都不能跟我们对话。）”  
> (Dave Barry，幽默作家)  
> 2. “I’ve noticed lately that the paranoid fear of computers becoming intelligent and taking over the world has almost entirely disappeared from the common culture. Near as I can tell, this coincides with the release of MS-DOS.”  
> (Larry DeLuca)  
> “我最近注意到，在共同文化中，那种对计算机变得智能化并最终掌控世界的妄想恐惧症几乎彻底消失了。据我所知，这跟MS-DOS的发布基本是同步的。”  
> (Larry DeLuca)  
> 3. “The question of whether computers can think is like the question of whether submarines can swim.”  
> (Edsger W. Dijkstra)  
> “计算机会不会思考这个问题就像问潜水艇会不会游泳一样。”  
> (Edsger W. Dijkstra，图灵奖获得者)  
> 4. “It’s ridiculous to live 100 years and only be able to remember 30 million bytes. You know, less than a compact disc. The human condition is really becoming more obsolete every minute.”  
(Marvin Minsky)  
> “活了一百年却只能记住30M字节是荒谬的。你知道，这比一张压缩盘还要少。人类境况正在变得日趋退化。”  
> (Marvin Minsky，人工智能研究的奠基人)  
> 
> ## Trust信任  
> 1. “The city’s central computer told you? R2D2, you know better than to trust a strange computer!”  
> (C3PO)  
> “这座城市的中央计算机告诉你的？R2D2，你不该相信一台陌生的计算机！”  
> (C3PO，星球大战中的翻译机器人)  
> 2. “Never trust a computer you can’t throw out a window.”  
> (Steve Wozniak)  
> “永远不要相信一台不能扔掉一扇窗户的计算机”  
> (斯蒂夫·沃兹尼亚克，苹果联合创始人)  
> 译者：暗指微软的WINDOWS操作系统  
>   
> ## Hardware硬件  
> 1. “Hardware: The parts of a computer system that can be kicked.”  
> (Jeff Pesis)  
> “硬件:计算机系统中可被踢的部分。”  
> (Jeff Pesis)  
>   
> ## Software软件  
> 1. “Most software today is very much like an Egyptian pyramid with millions of bricks piled on top of each other, with no structural integrity, but just done by brute force and thousands of slaves.”  
> (Alan Kay)  
> “今天大部分的软件都很像上百万块砖堆叠在一起组成的埃及金字塔，缺乏结构完整性，只能靠强力和成千上万的奴隶完成。”  
> (阿伦·凯，图灵奖获得者，面向对象创始人)  
> 2. “I’ve finally learned what ‘upward compatible’ means. It means we get to keep all our old mistakes.”  
> (Dennie van Tassel)  
> “我终于明白‘向上兼容性’是怎么回事了。这是指我们得保留所有原有错误。”  
> (Dennie van Tassel)  
>   
> ## Operating Systems操作系统  
> 1. “There are two major products that come out of Berkeley: LSD and UNIX. We don’t believe this to be a coincidence.”  
> (Jeremy S. Anderson)  
> “有两样重要产品出自伯克利：LSD和BSD。我们不相信这是个巧合。”  
> (Jeremy S. Anderson)  
> 译者：LSD是一种药力至强的迷幻剂，BSD-BSD（Berkeley Software Distribution，伯克利软件套件）是Unix的衍生系统  
> 2. “19 Jan 2038 at 3:14:07 AM”  
> (End of the word according to Unix–2^32 seconds after January 1, 1970)  
> “2038年1月19日，凌晨3点14分07秒”  
> (UNIX中的世界末日–1970年1月1号之后的2^32秒)  
> 译者：word跟world同音，UNIX用有符号整形数（WORD）表示时间，所以最多只能计时2^31秒，原文的2^32应为错误。  
> 3. “Every operating system out there is about equal… We all suck.”  
> (Microsoft senior vice president Brian Valentine describing the state of the art in OS security, 2003)  
> “每个操作系统都差不多… 我们都一样的烂。”  
> (微软的高级副总裁布莱恩·瓦伦蒂尼这样描述操作系统的安全状况，2003)  
> 4. “Microsoft has a new version out, Windows XP, which according to everybody is the ‘most reliable Windows ever.’ To me, this is like saying that asparagus is ‘the most articulate vegetable ever.’ ”  
> (Dave Barry)  
> “微软有出了个新版本，Windows XP,据大家说是‘有史以来最稳定的Windows’， 对我而言, 这就好像是在说芦笋是‘有史以来发音最清脆的蔬菜一样’ ”  
> (Dave Barry)  
>   
> ## Internet互联网  
> 1. “The Internet? Is that thing still around?”  
> (Homer Simpson)  
> “互联网？那个东西还在吗？”  
> (Homer Simpson)  
> 2. “The Web is like a dominatrix. Everywhere I turn, I see little buttons ordering me to Submit.”  
> (Nytwind)  
> “网络就像是个母夜叉。我每转到一处都会看见小个的按钮命令我提交。”  
> (Nytwind)  
> 译者注：Submit：提交，另一层意思是要求屈服  
> 3. “Come to think of it, there are already a million monkeys on a million typewriters, and Usenet is nothing like Shakespeare.”  
> (Blair Houghton)  
> “想想看吧，已经有一百万只猴子坐在一百万台打字机旁，可Usenet就是比不上莎士比亚。”  
> (Blair Houghton)  
>   
> ## Software Industry软件产业  
> 1. “The most amazing achievement of the computer software industry is its continuing cancellation of the steady and staggering gains made by the computer hardware industry.”  
> (Henry Petroski)  
> “计算机软件产业最为惊人的成就，是其持续不断地放弃硬件产业的惊人成果和稳定性。”  
> (Henry Petroski)  
> 2. “True innovation often comes from the small startup who is lean enough to launch a market but lacks the heft to own it.”  
> (Timm Martin)  
> “真正的创新经常来自于那些贴近市场、但无力拥有市场的的小型初创公司。”  
> (Timm Martin)  
> 3. “It has been said that the great scientific disciplines are examples of giants standing on the shoulders of other giants. It has also been said that the software industry is an example of midgets standing on the toes of other midgets.”  
> (Alan Cooper)  
> “人们常说，伟大的科学学科就像是站在其它巨人肩膀上的巨人。人们也说过，软件产业正如站在其他侏儒脚上的侏儒。”  
> (Alan Cooper，交互设计之父)  
> 4. “It is not about bits, bytes and protocols, but profits, losses and margins.”  
> (Lou Gerstner)  
> “这无关比特、字节和协议，而关乎利润和损益。”  
> (郭士纳，IBM前CEO)  
> 5. “We are Microsoft. Resistance Is Futile. You Will Be Assimilated.”  
> (Bumper sticker)  
> “我们是微软。反抗是徒劳的。你会被同化的。”  
> (保险杠贴纸)  
>   
> ## Software Demos软件演示  
> 1. “No matter how slick the demo is in rehearsal, when you do it in front of a live audience, the probability of a flawless presentation is inversely proportional to the number of people watching, raised to the power of the amount of money involved.”  
> (Mark Gibbs)  
> “不管演示在彩排的时候有多好，一旦在观众面前展示时，演示不出错的几率与观众人数成反比，与投入的金钱总额成正比。”  
> (Mark Gibbs)  
>   
> ## Software Patents软件专利  
> 1. “The bulk of all patents are crap. Spending time reading them is stupid. It’s up to the patent owner to do so, and to enforce them.”  
> (Linus Torvalds)  
> “专利大多数都是垃圾。浪费时间去阅读这些专利是愚蠢的。只有专利持有人才会这么干，还得强迫自己才会看。”  
> (Linus Torvalds，LINUX创始人)  
>   
> ## Complexity复杂性  
> 1. “Controlling complexity is the essence of computer programming.”  
> (Brian Kernigan)  
> “控制复杂性是计算机编程的本质。”  
> (Brian Kernigan)  
> 2. “Complexity kills. It sucks the life out of developers, it makes products difficult to plan, build and test, it introduces security challenges, and it causes end-user and administrator frustration.”  
> (Ray Ozzie)  
> “复杂性杀死一切。它把程序员的生活给搞砸了，它令产品难以规划、创建和测试，带来了安全挑战，并导致最终用户和管理员沮丧不已。”  
> (Ray Ozzie)  
> 3. “There are two ways of constructing a software design. One way is to make it so simple that there are obviously no deficiencies. And the other way is to make it so complicated that there are no obvious deficiencies.”  
> (C.A.R. Hoare)  
> “进行软件设计有两种方式。一种是让它尽量简单，让人看不出明显的不足。另一种是弄得尽量复杂，让人看不出明显的缺陷。”  
> (C.A.R. Hoare)  
> 4. “The function of good software is to make the complex appear to be simple.”  
> (Grady Booch)  
> “好的软件的作用是让复杂的东西看起来简单。”  
> (Grady Booch，UML创始人之一)  
>   
> ## Ease of Use易用性  
> 1. “Just remember: you’re not a ‘dummy,’ no matter what those computer books claim. The real dummies are the people who–though technically expert–couldn’t design hardware and software that’s usable by normal consumers if their lives depended upon it.”  
> (Walter Mossberg)  
> “不管那些计算机书籍如何宣称，只需记住，你并非‘傀儡’。真正的傀儡是那些无法设计出易于使用的硬件和软件的那些人，尽管他们是技术专家，因为这是普通消费者赖以生活的东西。”  
> (Walter Mossberg，科技专栏记者)  
> 2. “Software suppliers are trying to make their software packages more ‘user-friendly’… Their best approach so far has been to take all the old brochures and stamp the words ‘user-friendly’ on the cover.”  
> (Bill Gates)  
> “软件供应商在努力尝试让他们的软件更‘易于操作’… 迄今为止，他们最好的办法就是翻出所有的老手册，然后在封面盖上‘易于操作’这几个字。”  
> (比尔·盖茨)  
> 3. “There’s an old story about the person who wished his computer were as easy to use as his telephone. That wish has come true, since I no longer know how to use my telephone.”  
> (Bjarne Stroustrup)  
> “有个老套的故事说有人希望他的计算机能像他的电话机一样好用。他的愿望实现了，因为我已经不知道该如何使用自己的电话了。”  
> (Bjarne Stroustrup，C++之父)  
>   
> ## Users用户  
> 1. “Any fool can use a computer. Many do.”  
> (Ted Nelson)  
> “任何一个傻瓜都会用电脑。很多都会。”  
> (Ted Nelson)  
> 2. “There are only two industries that refer to their customers as ‘users’.”  
> (Edward Tufte)  
> “只有两个行业把客户称为‘用户’。”  
> (Edward Tufte，信息设计大师)  
> 译者注：一个是计算机设计，另一个是毒品交易，computer design and drug dealing  
>   
> ## Programmers程序员  
> 1. “Programmers are in a race with the Universe to create bigger and better idiot-proof programs, while the Universe is trying to create bigger and better idiots. So far the Universe is winning.”  
> (Rich Cook)  
> “程序员在跟宇宙赛跑，他们在努力开发出更大更好的傻瓜程序，而宇宙则努力培养出更大更好的白痴。到目前为止，宇宙领先。”  
> (Rich Cook)  
> 2. “Most of you are familiar with the virtues of a programmer. There are three, of course: laziness, impatience, and hubris.”  
> (Larry Wall)  
> “你们当中很多人都知道程序员的美德。当然啦，有三种：那就是懒惰、急躁以及傲慢。”  
> (Larry Wall，Perl发明者)  
> 3. “The trouble with programmers is that you can never tell what a programmer is doing until it’s too late.”  
> (Seymour Cray)  
> “程序员的问题是你无法预料他在做什么，直到为时已晚。”  
> (Seymour Cray，超级计算机之父)  
> 4. “That’s the thing about people who think they hate computers. What they really hate is lousy programmers.”  
> (Larry Niven)  
> “那就是这些自认为痛恨计算机的人的真实面目。他们实际上真正痛恨的是糟糕的程序员。”  
> (拉瑞·尼文，科幻作家)  
> 5. “For a long time it puzzled me how something so expensive, so leading edge, could be so useless. And then it occurred to me that a computer is a stupid machine with the ability to do incredibly smart things, while computer programmers are smart people with the ability to do incredibly stupid things. They are, in short, a perfect match.”  
> (Bill Bryson)  
> “很长时间以来我一直困惑不已，为什么一些又贵又先进的东西会一点用都没有。直到我突然想起，计算机不就是一台愚蠢之至却拥有难以置信的做聪明事能力的机器嘛，而程序员不就是聪明绝顶却拥有难以置信的干蠢事的能力的人嘛。一句话，他们简直就是天生绝配。”  
> (比尔·布莱森，旅游文学作家)  
> 6. “Computer science education cannot make anybody an expert programmer any more than studying brushes and pigment can make somebody an expert painter.”  
> (Eric Raymond)  
> “不像学学涂涂画画也能让某人成为专家级画家，计算机科学教育不会让任何人成为一名编程大师。”  
> (埃里克·雷蒙，开源运动领袖)  
> 7. “A programmer is a person who passes as an exacting expert on the basis of being able to turn out, after innumerable punching, an infinite series of incomprehensive answers calculated with micrometric precisions from vague assumptions based on debatable figures taken from inconclusive documents and carried out on instruments of problematical accuracy by persons of dubious reliability and questionable mentality for the avowed purpose of annoying and confounding a hopelessly defenseless department that was unfortunate enough to ask for the information in the first place.”  
> (IEEE Grid newsmagazine)  
> “一个程序员是经历以下事情后仍能证明自己是严格的专家的人：他可以历经数不清的捶打，可取材于无关紧要的文档，用上面的争议数据作出模糊假设，并以此计算出测微精度的无数片面理解的答案,并由一个不可靠、脑袋充满质疑、公开宣称要让一个倒霉透顶、没有指望、毫无防备,要求第一时间获得信息的部门狼狈不堪、令人生厌的人使用一台准确度有问题的仪器去实施。”  
> (IEEE网格新闻杂志)  
> 8. “A hacker on a roll may be able to produce–in a period of a few months–something that a small development group (say, 7-8 people) would have a hard time getting together over a year. IBM used to report that certain programmers might be as much as 100 times as productive as other workers, or more.”  
> (Peter Seebach)  
> “运气好的黑客能用几个月的时间 - 生产出一个小规模的开发团体（比如说，7-8人）历尽艰辛一起工作了一年多才能做出来的东西。IBM经常报告说某些程序员的生产力要比其它工人高百倍，甚至更多。”  
> (Peter Seebach，黑客)  
> 9. “The best programmers are not marginally better than merely good ones. They are an order-of-magnitude better, measured by whatever standard: conceptual creativity, speed, ingenuity of design, or problem-solving ability.”  
> (Randall E. Stross)  
> “最好的程序员跟好的程序员相比可不止好那么一点点。这种好不是一个数量级的，取决于标准怎么定：概念创造性、速度、设计的独创性或者解决问题的能力。”  
> (兰德尔·E·斯特劳斯，科技作家)  
> 10. “A great lathe operator commands several times the wage of an average lathe operator, but a great writer of software code is worth 10,000 times the price of an average software writer.”  
> (Bill Gates)  
> “伟大的车工值得给他几倍于普通车工的薪水，但一个伟大的软件代码作家，其价值则要等同于一个普通的软件写手的价格的1万倍。”  
> (比尔·盖茨)  
>   
> ## Programming编程  
> 1. “Don’t worry if it doesn’t work right. If everything did, you’d be out of a job.”  
> (Mosher’s Law of Software Engineering)  
> “就算它工作不正常也别担心。如果一切正常，你早该失业了。”  
> (Mosher的软件工程定律)  
> 2. “Measuring programming progress by lines of code is like measuring aircraft building progress by weight.”  
> (Bill Gates)  
> “靠代码行数来衡量开发进程就好比用重量来衡量飞机制造的进度。”  
> (比尔·盖茨)  
> 3. “Writing code has a place in the human hierarchy worth somewhere above grave robbing and beneath managing.”  
> (Gerald Weinberg)  
> “写代码的社会地位比盗墓的高，比管理的低。”  
> (杰拉尔·德温伯格，软件与系统思想家)  
> 4. “First learn computer science and all the theory. Next develop a programming style. Then forget all that and just hack.”  
> (George Carrette)  
> “首先学习计算机科学及理论。接着形成自己编程的风格。然后把这一切都忘掉，尽管改程序就是了。”  
> (George Carrette，杰出软件工程师,开源推广者)  
> 5. “First, solve the problem. Then, write the code.”  
> (John Johnson)  
> “先解决问题再写代码。”  
> (John Johnson)  
> 6. “Optimism is an occupational hazard of programming; feedback is the treatment.”  
> (Kent Beck)  
> “乐观主义是编程行业的职业病；用户反馈则是治疗方法。”  
> (Kent Beck)  
> 7. “To iterate is human, to recurse divine.”  
> (L. Peter Deutsch)  
> “迭代者为人，递归者为神。”  
> (L. Peter Deutsch)  
> 8. “The best thing about a boolean is even if you are wrong, you are only off by a bit.”  
> (Anonymous)  
> “布尔值最好的一点是，就算你错了，也顶多错了一位而已。”  
> (无名氏)  
> 9. “Should array indices start at 0 or 1? My compromise of 0.5 was rejected without, I thought, proper consideration.”  
> (Stan Kelly-Bootle)  
> “数组的下标是从0开始好还是从1开始好呢？我的0.5的折衷方案，以我之见，没有经过适当考虑就被否决掉了。”  
> (Stan Kelly-Bootle)  
>   
> ## Programming Languages编程语言  
> 1. “There are only two kinds of programming languages: those people always bitch about and those nobody uses.”  
> (Bjarne Stroustrup)  
> “只有两种编程语言：一种是天天挨骂的，另一种是没人用的。”  
> (Bjarne Stroustrup，C++之父)  
> 2. “PHP is a minor evil perpetrated and created by incompetent amateurs, whereas Perl is a great and insidious evil perpetrated by skilled but perverted professionals.”  
> (Jon Ribbens)  
> “PHP是不合格的业余爱好者创建的，他们犯做了个小恶；Perl是娴熟而堕落的专家创建的，他们犯了阴险狡诈的大恶。”  
> (Jon Ribbens)  
> 3. “The use of COBOL cripples the mind; its teaching should therefore be regarded as a criminal offense.”  
> (E.W. Dijkstra)  
> “COBOL的使用摧残大脑；其教育应被视为刑事犯罪。”  
> (E.W. Dijkstra)
> 4. “It is practically impossible to teach good programming style to students that have had prior exposure to BASIC. As potential programmers, they are mentally mutilated beyond hope of regeneration.”  
> (E. W. Dijkstra)  
> “把良好的编程风格教给那些之前曾经接触过BASIC的学生几乎是不可能的。作为可能的程序员，他们已精神残废，无重塑的可能了。”  
> (E. W. Dijkstra)  
> 5. “I think Microsoft named .Net so it wouldn’t show up in a Unix directory listing.”  
> (Oktal)  
> “我想微软之所以把它叫做.Net，是因为这样它就不会在Unix的目录里显示出来了。”  
> (Oktal)  
> 6. “There is no programming language–no matter how structured–that will prevent programmers from making bad programs.”  
> (Larry Flon)  
> “没有一种编程语言能阻止程序员写出糟糕的程序来，不管这种语言结构有多良好。”  
> (Larry Flon)  
> 7. “Computer language design is just like a stroll in the park. Jurassic Park, that is.”  
> (Larry Wall)  
> “计算机语言设计犹如在公园里漫步。我是说侏罗纪公园。”  
> (Larry Wall)  
>   
> ## C/C++  
> 1. “Fifty years of programming language research, and we end up with C++?”  
> (Richard A. O’Keefe)  
> “搞了50年的编程语言的研究，我们难道就以C++告终啦？”  
> (Richard A. O’Keefe)  
> 2. “Writing in C or C++ is like running a chain saw with all the safety guards removed.”  
> (Bob Gray)  
> “写C或者C++就像是在用一把卸掉所有安全防护装置的链锯。”  
> (Bob Gray)  
> 3. “In C++ it’s harder to shoot yourself in the foot, but when you do, you blow off your whole leg.”  
> (Bjarne Stroustrup)  
> “在C++里你想搬起石头砸自己的脚更为困难了，不过一旦你真的做了，整条腿都要报销。”  
> (Bjarne Stroustrup)  
> 4. “C++ : Where friends have access to your private members.”  
> (Gavin Russell Baker)  
> “C++ : 友人可造访你的私有成员之地也。”  
> (Gavin Russell Baker)  
> 译者：Friends：C++的友元，是一种定义在类外部的普通函数，但它需要在类体内进行说明，为了与该类的成员函数加以区别，在说明时前面加以关键字friend。友元不是成员函数，但是它可以访问类中的私有成员。友元的作用在于提高程序的运行效率，但是，它破坏了类的封装性和隐藏性，使得非成员函数可以访问类的私有成员。 
> 5. “One of the main causes of the fall of the Roman Empire was that–lacking zero–they had no way to indicate successful termination of their C programs.”  
> (Robert Firth)  
> “罗马帝国灭亡的其中一个主要原因是他们没有0 - 这样他们就没法给自己的C程序指明成功退出的路径了。”  
> (Robert Firth)  
>   
> ## Java  
> 1. “Java is, in many ways, C++–.”  
> (Michael Feldman)  
> “Java从许多方面来说就是C++–。”  
> (Michael Feldman)  
> 2. “Saying that Java is nice because it works on all OSes is like saying that anal sex is nice because it works on all genders.”  
> (Alanna)  
> “说Java好就好在运行于多个操作系统之上，就好像说肛交好就好在不管男女都行。”  
> (Alanna)  
> 3. “Fine, Java MIGHT be a good example of what a programming language should be like. But Java applications are good examples of what applications SHOULDN’T be like.”  
> (pixadel)  
> “好吧，Java也许是编程语言的好榜样。但Java应用则是应用程序的坏榜样。”  
> (pixadel)  
> 4. “If Java had true garbage collection, most programs would delete themselves upon execution.”  
> (Robert Sewell)  
> “要是Java真的有垃圾回收的话，大部分程序在执行的时候就会把自己干掉了。”  
> (Robert Sewell)  
> ##Open Source开源  
> 1. “Software is like sex: It’s better when it’s free.”  
> (Linus Torvalds)  
> “软件就像性事：免费/自由更好。”  
> (Linus Torvalds)  
> 2. “The only people who have anything to fear from free software are those whose products are worth even less.”  
> (David Emery)  
> “唯一对免费软件感到害怕的人，是自己的产品还要不值钱的人。”  
> (David Emery)  
>   
> ## Code代码  
> 1. “Good code is its own best documentation.”  
> (Steve McConnell)  
> “好代码本身就是最好的文档。”  
> (Steve McConnell)  
> 2. “Any code of your own that you haven’t looked at for six or more months might as well have been written by someone else.”  
> (Eagleson’s Law)  
> “你自己的代码如果超过6个月不看，再看的时候也一样像是别人写的。”  
> (伊格尔森定律)  
> 3. “The first 90% of the code accounts for the first 90% of the development time. The remaining 10% of the code accounts for the other 90% of the development time.”  
> (Tom Cargill)  
> “前面90%的代码要占用开发时间的前90%。剩下的10%的代码要占用开发时间的另一90%。”  
> (Tom Cargill)  
>   
> ## Software Development软件开发  
> 1. “Good programmers use their brains, but good guidelines save us having to think out every case.”  
> (Francis Glassborow)  
> “好的程序员会用脑，但是好的向导救我们于样样都要想到。”  
> (Francis Glassborow)  
> 2. “In software, we rarely have meaningful requirements. Even if we do, the only measure of success that matters is whether our solution solves the customer’s shifting idea of what their problem is.”  
> (Jeff Atwood)  
> “在软件里面，我们鲜有有意义的需求。就算有，衡量成功的唯一尺度也取决于我们的解决方案是否解决了客户对问题是什么的观念的转变。”  
> (Jeff Atwood)  
> 3. “Considering the current sad state of our computer programs, software development is clearly still a black art, and cannot yet be called an engineering discipline.”  
> (Bill Clinton)  
> “想想我们计算机程序的糟糕现状吧，很显然软件开发仍是黑箱艺术，还不能称之为工程学科。”  
> (Bill Clinton，前美国总统)  
> 4. “You can’t have great software without a great team, and most software teams behave like dysfunctional families.”  
> (Jim McCarthy)  
> “没有伟大的团队就没有伟大的软件，可大部分的软件团队举止就像是支离破碎的家庭。”  
> (吉姆·麦卡锡，微软VC++总监)  
>   
> ## Debugging调试  
> 1. “As soon as we started programming, we found to our surprise that it wasn’t as easy to get programs right as we had thought. Debugging had to be discovered. I can remember the exact instant when I realized that a large part of my life from then on was going to be spent in finding mistakes in my own programs.”  
> (Maurice Wilkes discovers debugging, 1949)  
> “一旦我们开始编程，就会惊讶地发现让程序正常没想象中那么简单。调试不可避免。那一刻我认记忆犹新，当时我就意识到，从今往后我生活的大部分时间都要花在寻找自己程序的错误上面了。”  
> (莫里斯·威尔克斯 调试探索, 1949)  
> 2. “Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are–by definition–not smart enough to debug it.”  
> (Brian Kernighan)  
> “调试难度本来就是写代码的两倍。因此，如果你写代码的时候聪明用尽，根据定义，你就没有能耐去调试它了。”  
> (Brian Kernighan)  
> 3. “If debugging is the process of removing bugs, then programming must be the process of putting them in.”  
> (Edsger W. Dijkstra)  
> “如果调试是除虫的过程，那么编程就一定是把臭虫放进来的过程。”  
> (Edsger W. Dijkstra)  
>   
> ## Quality质量   
> 1. “I don’t care if it works on your machine! We are not shipping your machine!”  
> (Vidiu Platon)  
> “我才不管它能不能在你的机器上运行呢！我们又没装到你的机器上！”  
> (Vidiu Platon，罗马尼亚的微软最佳学生合作伙伴MSP)  
> 2. “Programming is like sex: one mistake and you’re providing support for a lifetime.”  
> (Michael Sinz)  
> “编程就像性一样：一时犯错，终生维护。”  
> (Michael Sinz)  
> 3. “There are two ways to write error-free programs; only the third one works.”  
> (Alan J. Perlis)  
> “有两种写出无错程序的办法；只有第三种有用。”  
> (Alan J. Perlis)  
> 4. “You can either have software quality or you can have pointer arithmetic, but you cannot have both at the same time.”  
> (Bertrand Meyer)  
> “软件质量与指针算法不可兼得。”  
> (Bertrand Meyer)  
> 5. “If McDonalds were run like a software company, one out of every hundred Big Macs would give you food poisoning, and the response would be, ‘We’re sorry, here’s a coupon for two more.’ “  
> (Mark Minasi)  
> “如果麦当劳像软件公司那样运作的话，每一百个巨无霸就会有一个令你食物中毒，而他们的回应是，‘真对不起，这是一张额外附送两个的赠券。’ ”  
> (Mark Minasi)  
> 6. “Always code as if the guy who ends up maintaining your code will be a violent psychopath who knows where you live.”  
> (Martin Golding)  
> “永远要这样写代码，好像最终维护你代码的人是个狂暴的、知道你住在哪里的精神病患者。”  
> (Martin Golding)  
> 7. “To err is human, but to really foul things up you need a computer.”  
> (Paul Ehrlich)  
> “是人都会犯错，不过要想把事情彻底搞砸还得请电脑出马。”  
> (Paul Ehrlich)  
> 8. “A computer lets you make more mistakes faster than any invention in human history–with the possible exceptions of handguns and tequila.”  
> (Mitch Radcliffe)  
> “计算机比人类历史上的任何发明都更快速地导致你犯更多的错误–可能除了手枪和龙舌兰酒是例外。”  
> (Mitch Radcliffe)  
>   
> ## Predictions预测  
> 1. “Everything that can be invented has been invented.”  
> (Charles H. Duell, Commissioner, U.S. Office of Patents, 1899)  
> “能发明的东西都发明出来了。”  
> (查尔斯·杜埃尔, 美国专利局局长，1899年)  
> 2. “I think there’s a world market for about 5 computers.”  
> (Thomas J. Watson, Chairman of the Board, IBM, circa 1948)  
> “我认为全球市场约需5台计算机。”  
> (托马斯·沃森, IBM董事长, 约1948年)  
> 3. “It would appear that we have reached the limits of what it is possible to achieve with computer technology, although one should be careful with such statements, as they tend to sound pretty silly in 5 years.”  
> (John Von Neumann, circa 1949)  
> “看上去我们已经到达了利用计算机技术可能获得的极限了，尽管下这样的结论得小心，因为不出五年这听起来就会相当愚蠢。”  
> (约翰·冯·诺伊曼,约1949年)  
> 4. “But what is it good for?”  
> (Engineer at the Advanced Computing Systems Division of IBM, commenting on the microchip, 1968)  
> “但这又有什么好处呢？”  
> (IBM先进计算机系统部的工程师对微芯片的评论, 1968年)  
> 5. “There is no reason for any individual to have a computer in his home.”  
> (Ken Olson, President, Digital Equipment Corporation, 1977)  
> “我们没有理由让每一个人在家都拥有一台电脑。”  
> (肯·奥尔森,数据设备公司（DEC）总裁，1977年)  
> 6. “640K ought to be enough for anybody.”  
> (Bill Gates, 1981)  
> “640K对每一个人来说都已足够。”  
> (比尔·盖茨,1981年)  
> 7. “Windows NT addresses 2 Gigabytes of RAM, which is more than any application will ever need.”  
> (Microsoft, on the development of Windows NT, 1992)  
> “Windows NT的RAM寻址空间可达2G，这比任何应用程序所需都要多。”  
> (微软, 谈及Windows NT的开发时所言, 1992年)  
> 8. “We will never become a truly paper-less society until the Palm Pilot folks come out with WipeMe 1.0.”  
> (Andy Pierson)  
> “我们永远也无法真正成为无纸化社会，直到掌上电脑一族发布擦我1.0（WipeMe 1.0）为止。”  
> (安迪•皮尔逊，商界领袖)  
> 译者注：意思是说难道你大便不用纸吗？
> 9. “If it keeps up, man will atrophy all his limbs but the push-button finger.”  
> (Frank Lloyd Wright)    
> “长此以往，除了按键的手指外，人类的肢体将全部退化。”  
> (弗兰克•劳埃德•赖特，建筑师)

------------

> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_80x15.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> [![知识共享许可协议](https://res.zhangkai.xin/pic/license/BY-NC-SA_88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)
> 
> 本作品采用[知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)进行许可。