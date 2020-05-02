---
title: 题解 T27533 【[开昕生日愚人题#6]无规律数列】
date: 2018-04-18 15:08:00
tags: 
  - 题解
---
[LuoguT27533](https://www.luogu.org/problemnew/show/T27533):

拿到这组数蒙了？为了方便大家找规律，我在出样例时特意给大家又多往后延伸了一位n==6时的样例。

0pts：知道我的题【U22412 [PP游戏#5 种树游戏（Tree Game）](https://www.luogu.org/problemnew/show/U22412)】的朋友可能会根据$3\leq n \leq 6$时的数列$\begin{Bmatrix}1&1&2&3& \cdots \end{Bmatrix}$以为是我PP游戏#5中n==2时的斐波那契数列然后一看$n\leq16$就打表输出，结果就全WA了。

40pts：在这里科普的是，这个数列真的是个毫无规律的数列，但也并不是完全没有，真正的做法是百度一下这个数列，然后就出来了一个[视频链接](http://v.youku.com/v_show/id_XMTMxNzQ1Nzg2OA==.html)，在这个视频中，国外YouTube的numberphile数学家讲解了这个数列是素纽结的结数序列，并把这个数列给到了$n\leq10$时的值，打表提交即可得到40pts。

70pts：显然这题数据点是$7\leq n\leq16$，所以发动广大网友的力量，我们成功的在某贴吧中找到了$n\leq13$时的值，打表提交即可得到70pts。

100pts：题解到这里，不仅会有人会问，都已经翻遍网络了，还没有AC，PM你的后3个点是不是故意不让人过的所以乱造的数据啊？这里我要回复：不，那是因为你没有访问维基百科找[Prime_knot素纽结](https://en.wikipedia.org/wiki/Prime_knot)，这才是真正的答案来源，可以看到连维基百科都只把数列给到$n\leq16$，说明这是多么的无规律，所以~~毒瘤的~~我当然不会放过了，最后打表提交即可AC。