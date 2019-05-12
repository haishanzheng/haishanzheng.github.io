---
title:  "Microsoft Money 2004 入门 报表篇"
date:   2004-05-11 17:50:20 +0800
---

前面搞了这么多事，每天输入累得半死，录入了数据干吗用？出报表。Money的报表功能异常强大，只有你想不到，没有Money做不到。报表功能是我抛弃PMT选择Money的主要原因:)，虽然我不用报表做什么决策。  

Money的基本报表包括7个系列，每个系列数差不多10个，有图为证。  
![](/images/2011/money/report.gif)  
咳咳，你应该每个都去点一下。除了基本报表，还可以有My Favorites收藏夹。所有的报表都可以自定义，可以更改标题，行列类型，是用明细还是饼图还是柱状图，平面还是3D，时间范围，涉及帐户、收款人，数量。如果你有Sql编程基础，你已经想到了，SELECT xxx FROM tb_Transcation WHERE xxx = xxx AND xxx BETWEEN (xxx, xxx) GROUP BY xxx ORDER BY xxx xxx。所有的这些xxx都是可自定义的。虽然你可以直接在7个基本系列里做修改，但是我推荐还是把基本报表加入到Favorite再做修改。  

产生自定义报表有2种方法，一个是在Favorite里修改，一种是在看到有Create a Report的地方点击生成Report然后Save to Favorite再修改。不要问我在哪里找到Create a Report，多用用并仔细观察，你有机会看到的。以下以一个修改作为例子。  

在Reports Gallery的第一个系列第一个报表是Spending by Category，点开，这个报表是以明细科目显示你消费状况，这样太细，把Food: Dining out，Food: Milk，Food: Fruit都列出来了，也就是说其实是Spending by Subcategory，所以你（其实是我）想做2个报表，一个是Spending by Category，一个是Spending by Subcategory。首先在这个Spending by Category，点击Favorites->Add to Favorites或者右键，修改名字Spending by Subcategory，选择Customize...，把title改为Spending by Subcategory。继续在这个Spending by Category，点击Favorites->Add to Favorites，修改名字Spending by Category，然后选择左边的Customize...，在Rows & Columns的Rows，设置为Categories。这样就可以了。  

报表你可以输出成bmp，txt，cvs，xml等多种格式。  

最后提一下恩格儿系数（Engel's Coefficient），食物消费支出占生活消费支出的比重，恩格儿系数越高，说明你生活水平越低，如果把钱都花在吃上面，不管你每天花10万，生活也可够悲惨的。不用自卑，有段时间我的恩格儿系数就是1咳咳。  

