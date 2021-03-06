---
title: "闲谈设计模式"
date: 2020-05-09T09:54:18+08:00
---

今天差不多把《head first 设计模式》这本书看完了，总算是对设计模式有了最基本的了解，想想自己对设计模式的认识变化还蛮大的，就记录下来这些改变。  



#### 知道设计模式

* 第一次知道设计模式这个名词，应该是半年前的事情了。匆匆买了一本 GoF 的设计模式回来，一看那么薄，感觉也就一天的事情。后来翻了翻，23 种模式，又都没什么了解，放弃之：**等用到的时候再看吧**。

* 因为**等到用到的时候再看吧**，因而一直没用设计模式来组织自己的代码。当时自己用 Spring 的 RestTemplate 写通讯接口，通讯接口约定格式经常发生变化，做完了它在变，没做完它也在变，设计模式根本帮不上忙(现在想想也的确如此)，因而认为设计模式不过是落伍的架子而已。

* 大概 8 月的时候，前一个项目完成了一个里程碑。去了另外一个项目做一些基础的开发。由于是 show 性质的代码，必须要用设计模式。无奈看一下 WIKI，加一些东西往里面。策略模式比较简单，就拿这个开刀了，实现了几个可以互换的算法，供客户端按需调用。感觉设计模式还是有那么点意思的。



#### 不屑设计模式

* 对设计模式最反感的地方是一群人(大多是只会 Java 的人)如同膜拜圣经一样膜拜设计模式(应该是有过之而无不及，毕竟信徒不会强迫别人信教，邪教除外)，在我看来，不过就是那么回事而已————因语言本身抽象能力的落后或 IDE 功能不够强大导致一种畸形的代码堆叠方式。

* 最明显的例子莫过于 goto，在汇编中，goto 对应的大概是 jmp。jmp 在逆向中经常用来做爆破的不二选择，在汇编中的优雅跳转也不是 jne 等等能替代的。到了 C 语言，抽象能力得到提升了，代码的定义终于出来了点端倪：

1. 能够让人阅读的东西
2. 刚好又可以在机器上运行

* goto 产生的面条式代码破坏了 1 (你是 jack 但我不是 Lucy，凭什么你 jump，我也要 jump)，随便乱跳打乱了流水线的调度，影响了 2 的效率。尽管 goto 还有很多奇妙的实现方式，但是已罪名深重的被贴上标签"有害" 。"goto 有害"大概不需要讨论了，因为大部分 goto 垃圾代码都是新手写的，只要唬住了他们就行，那些高手大牛如 Knuth 自然能驾驭 goto，自然不会随波逐流。

* 后来就学习一种比较快乐的语言 Ruby， 读 Matz 的书：《松本行弘的程序世界》。Matz 说道：

> 关于设计模式的书籍刚出现的时候，我最初的印象是“夸夸其谈，其实都是些理所当然的内容”。《设计模式》一书所列举的 23 种模式，很多都是经常使用，并不怎么罕见的模式。而且，还含有很多在 C++ 及 Java 那样的静态语言中虽然有效，但在 Smalltalk 及 Ruby 中却并没有多大意义的模式。

  > 但过了一阵子我有了更深刻的认识。设计模式的本质，并不是介绍至今没有用过的新模式，而是通过给屡屡使用过的模式起一个合适的名字，从而提供了设计时的词汇。


* 看到 Matz 对设计模式有了更深刻的认识，不禁也对设计模式有了好奇，想着自己必须要好好学习一下。虽说讨厌 Java，但是 Java 变成 Cobol 估计也要十年时间了。那就认真读一读设计模式吧，于是挑选了《head first 设计模式》(因为看这个入门真的很容易，至少自以为入门)。



#### 认识设计模式

* 《head first 设计模式》看完之后，对设计模式终于有了一个完整的印象————中医。

1. 是药三分毒。设计模式强大的扩展能力是用拆成碎片的类为代价的。这种能力的权衡是一个太严重的问题了，刚入行的中医会经常犯**这药没作用**，**这毒里面还有药！**。当然，为了安全以及名声考虑，刚入行的中医只会犯"这药没作用"的错误。于是，各种代码里面充斥着设计模式————10 个人去跑接力赛，8 个人拿着棒子就递给别人：我负责山地跑的，来这里是为了害怕突然地震造山了你们跑不了；全程只有两个人在跑。

2. 没有标准。OCP 衍生出多个可实现原则，矛盾也就在这里。当初孔子为了吹嘘尧舜两圣人，说出啥事了都是舜一去就搞定了，三次三搞！韩非子就在这里发现了矛盾：尧真牛逼，会出事？舜真牛逼，会年年有事？你们这些儒家啊，牛逼的不能是人，得有规矩，是个白痴丢那里都能正常的规矩。设计模式中有太多似是而非的模式，实际情况又不是教科书，到时候如何实现，和个人经验能力挂钩。活到老，学到老，如果是说语言，那一定是 C++；如果是方法，那一定是设计模式。

3. 在西医传来之前，中医对保证身体健康，应该是做了不少贡献的。西医的器材条件(至少显微镜吧)，以及一整套完整的方法论，出现总是要一段时间的。在这段时间唯一能做的，就是用中医"保命"，看看学术界(西医)究竟有没有做出来啥成果能产生革命性影响。



#### 过度设计？

* 最初看设计模式，因为有抵触心理，总是会想到过度设计这个词。但有时候自认为的过度，只不过是自认为而已。比如前段时间写一个给定两个日期算日期间的工作日，stackoverflow 之，看到这个[答案](http://stackoverflow.com/questions/4600034/calculate-number-of-weekdays-between-two-dates-in-java),来自 [Shengyuan Lu](http://stackoverflow.com/users/290629/shengyuan-lu)：

  - Solution without loop:

```java
        static long days(Date start, Date end){
        

        Calendar c1 = Calendar.getInstance();
        c1.setTime(start);
        int w1 = c1.get(Calendar.DAY_OF_WEEK);
        c1.add(Calendar.DAY_OF_WEEK, -w1);
          
        Calendar c2 = Calendar.getInstance();
        c2.setTime(end);
        int w2 = c2.get(Calendar.DAY_OF_WEEK);
        c2.add(Calendar.DAY_OF_WEEK, -w2);
        
        long days = (c2.getTimeInMillis()-c1.getTimeInMillis())/(1000*60*60*24);
        long daysWithoutSunday = days-(days*2/7);
          
        return daysWithoutSunday-w1+w2;
        }
```

* 漂亮，不用循环，copy 之，随便挑几个工作日看看结果是否正确，恩，正确！好方法，利用周六对齐来进行减法，太美妙了，简直要写对齐之美来赞美了。随便再丢组数据测试下，嗯？结果不对！周日周六在进行周六对齐的时候，多减去或增加了值。是不是要分析情况进行几个特殊值判断来校正补偿值，只有周六周日会出现这种情况，也就四种状态，同周六同周日不用校正，就俩了！

* 在一个算法上面补两个坑，太丑陋了。两个时间段差值不会超过 30，为何不用可读性更好，正确性更有保障的简单直白的循环？

* 1. 两个日期都是工作日，无循环算法可以正常工作。

* 2. 两个日期随意，照顾可读性和优雅，重新造一个循环算法是比较好的选择。

* 问题是，在一年内我们都用的工作日作为输入，我们采用循环算法，算是过度设计么？不需要讨论一年之后的情况，因为我们无法预料。

* 我认为，直接采用循环不算是过度设计。的确现在有很多真正的过度设计，但是在基本同样的工作量下，选择一个有更多余地的设计，不算是过度设计。因为设计必然是要考虑到将来的扩展的，如果说有余地的设计是过度设计的话，那一切设计本身就是过度设计。



#### 在黑完之后

* 设计模式将来必然会退出吧，如同现在基本没有人再去黑 goto了；然而，计算机界的很多问题，采用的思想都不变，除非冯·诺依曼倒塌了。将来的新问题，或许用设计模式的思想可以去解决一些，如同 Go 现在又引入了 goto。更何况设计模式还能解决当下的问题。而"**等到用到的时候再看吧**"只能骗骗自己了。

* 希望在半年内，能把设计模式单独列出来，补在博客上讲解给自己。