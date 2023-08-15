---
title:  "Microsoft Money 2004 入门 记帐篇"
date:   2004-04-10 21:33:49 +0800
---

前面介绍的都是准备工作，这么多天才开始写记帐篇，你应该有自己摸索了吧？  

“你在跟谁说话？没人看你的文章yeah？”  

咳咳，不管有没有人看，继续。选择Account List，选择一个账号，你看到的是。。。一片空白，那是你没有输入任何数据，窗体上半部分是交易明细，下半部分是输入筐，输入筐有3个选项卡，对应Cash类型是Spend、Receive、Transfer，Bank类型是Withdrawal、Deposit、Transfer，负债类型是Increase、Decrease、Transfer。真够乱的，其实你只需记住，如果帐户的钱减少了，就添写在第一个tab内，增加了就添写在第二个，如果是帐户之间的金钱来往，就添写在Transfer内（因为你的钱在多个卡内转来转去，总资产不变）。  

举几个录入场景：  

我用工行工资卡取款100块。  
选择Cash，Transfer，New，From 工行工资卡，To 现金，Amount 100，其他可以不填，Enter。  

我用现金买了盒依利牛奶。  
选择Cash，Spend，Pay to可填可不填，填了以后可以查看报表Spending by Payee，可以统计每年给胖哥沙锅送多少钱呵呵。Category，会计科目，选择Food: Milk，Amount 2.2，Enter。  

在路上拣到钱1毛。  
选择Cash，Receive，Category，Other Income : Gifts Received，拣到钱不能算收入，也没地方好放，就放在这儿，当是上帝给你的礼物，From God，Amount 0.1，Enter。  

遇到Beta，被借了10元。  
选择Cash，Transfer，New，From Cash，To 应收借款，Pay to，如果要trace和Beta之间的金钱来往，可以填入，或者记录在Memo里，Memo，Beta借我钱，（恶狠狠）给我记着。Amount，10。Enter。  

每月10号查工行卡，发现工资进来了。1567元。  
选择工行卡，Deposit，Category，Wages & Salary : Gross pay，Amount 1567，真少呀。。。嘟，Money弹出一个筐，建议你选上Make recurring，什么是Make recurring？下次再详细讲解，make recurring功能非常强大，Cash Flow现金流，Budget预算，Bills & Deposits等等都要从make recurring采集数据，你会很奇怪，为什么我上面那些都不会弹出这个筐，只有这个交易Money提醒我选上make recurring。。。咳咳，Money有内部设定，对于每个月来一次的东西。。。不要想歪了，比如Category为Bill类型（电费，水费），Gross pay等等的交易他才会提醒。不过我们现在可以不管，有后悔药。  

或许你还看到了Category右边的Split，这个是为复式记帐法准备的，比如你在Wal-Mart购物，买了一件300的上衣，50块钱的食物和20块钱的日用品，你通过刷卡付款，如果你分别入帐，那你的交易明细就会和银行寄给你的帐单不同。所以你可以这样记帐：  
选择信用卡，tab 1，New，Pay to Wal-Mart，Split，Category Cloth，Amount 300，Category Food，Amount 50，Category Commodity，Amount 20，Done，则Amount自动填入370，Category为Split/Multiple Categories，是不是很方便？咳咳。  

录入了数据干吗用？出报表，做预算，做计划，对似水流年的一个log。下表。  

附录入场景图一张，这个画面是从哪里来的？你以后就知道了。  
![](/images/2011/money/god.gif)  


## 评论

*****
**郑海山** 在 *2004-05-11 16:34:30* 说: 由于你是用2个account之间transfer，所以category就没有用处了，可以考虑使用classification来记录是差旅费还是饮食费用。

*****
**郑海山** 在 *2004-05-11 16:32:26* 说: 你希望达到什么目的？不要告诉我你想记得很清楚就是目的。这里提些建议，单独一个“待报销”帐户，每次都是Cash和待报销之间转帐。利用Money带的Flag for Follow-up或者Mark as clear这些来记录是否已经报帐。不知道这样可否？


*****
**匿名** 在 *2004-05-10 18:18:38* 说: 我也用了一年多的ms money 2003 了，也是没有用好make reccuring，还有我是一个长期出差人员，报销款项是我财务管理里很重要的一部分，可是我这一部分的帐记得很混乱，不知道各位有没有好的经验介绍

*****
**匿名** 在 *2004-04-15 23:16:36* 说: 我用money好久了，却从来没有用过Make recurring，一定要好好学学：）

我是从财智开始使用理财软件的，也是从那时开始感受到理财的重要。公平的说，用国产软件的水平来要求，财智做的算是那样了。跟MS的东西比，肯定是不能比。

不过，一般人估计不习惯用ms money，觉得太复杂。也许与中国人没有从小受到科学理财的教育有关。

*****
**郑海山** 在 *2004-04-11 11:52:24* 说: 不要跟我谈财智，农民才用的软件，模仿Money不太成功，界面土死了，功能混乱，表面的东西都这么不专业，程序内代码估计就更可怕了，做为一个程序员，我不敢使用这个软件。

不过他适合国情，可以网络下载建行和招行的交易明细，从国内的环境出发，还是一个不错的软件，其他人可以尝试着使用他。

*****
**匿名** 在 *2004-04-11 02:11:23* 说: 也还不错的。功能齐备，还简单。

靠，五百年过去了，这里还是不让注册。。

――大头

