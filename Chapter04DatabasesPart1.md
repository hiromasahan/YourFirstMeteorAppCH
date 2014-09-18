4 数据库（第一部分）

写一本技术书籍的困难之一是决定何时介绍想法。通常的做法是第一个谈创建界面，它有两个主要的好处：

1，您可以立即在你的浏览器看到东西。 
2，你觉得你进步较快。 

但它也导致了几个只会短期改变的问题，他们通过用户页面继续时：

1，当你不熟悉后端（数据库等）在发生什么的时候，很难真正掌握任何有关前端（界面）的问题。 
2，如果我们首先讨论前端，我们无论如何也要在下一章讨论，否则使你感觉我们现在的进展嘎然而止。

基于这些原因，我们不会再讨论界面。相反，我们将通过讨论数据库开始。如果仅凭这句话将让你想要入睡，很正常，但如果我们花几分钟的时间讨论在后端发生了什么事情，你会在这本书的其余部分夯实基础。 

4.1 MongoDB与SQL对比
如果你曾经安装的WordPress，使用phpMyAdmin的，或用PHP 创建了一个Web应用程序，那你一定你接触过一个SQL数据库。 

默认情况下，每一个Meteor的Project都有自己的数据库。不需要安装或配置。在project建立的时候数据库也建立了。然而这不是一个SQL数据库，相反，它是MongoDB数据库。

如果你从来没有遇到过的MongoDB之前，你可能会有点担心，但没有必要担心。 MongoDB数据库是和SQL数据库有些不同，但是，对于一个初学者这些差异很小。 

就目前而言，你需要知道两件事情：

首先，能和Meteor匹配的没有其他类型的数据库。如果你想使用一个SQL数据库，目前来说是不可能的。在未来可能会有其他选择，但MongoDB的确是一个不错的默认选项：

1，速度非常快。 
2，它的可扩展性。 
3，它很容易学习。 

所以，即使你以前从来没有接触过，任何数据库经验应该能足够迅速获得这些基础知识。 
其次，MongoDB使用不同的词语来形容熟悉的概念。我们不会，比如，用像表，行或列这样的词来表述，但其概念基本上是相同的。 
你可以看到这个表中的不同之处： 
           
           SQL                         MongoDB

           database                    database
           table                       colelection
           row                         document
           column                      field
           primary key                 primary key

这是当然，用不熟悉的词描述熟悉的概念的确有些别扭，但我会在学习过程中提醒你它们是什么，到最后你就能记得非常牢靠了。 

4.2 Collections

Leaderboard应用程序的核心功能是球员名单。如果没有的球员名单，我们不能在此基础上创造任何价值。这使得名单成为我们开始开发之旅的突破口 - 从我们的应用程序“中间”开始向外移动，朝着更精细的细节的进发。

但是，我们在哪里存放球员的数据？一般的回答是，“在数据库中。”

更精确和有用的回答是，“在一个collection中”，并且如之前对比的表所示，collection相当于一个SQL的table。 

如果我们在Meteor重建WordPress，我们会创造文章的collection，用户的collection，以及评论的collection。我们将针对每种类型的数据都建立相应的collection。因为我们是一个创建这个Leaderboard的应用程序，所以我们将要创建的球员的collection。 

要做到这一点，打开JavaScript文件，并在顶部写入以下语句（在isClient和isServer条件句外）： 

new Meteor.Collection（'players'）; 

在这里，我们创建了一个名为“players”的collection，但你可以把它叫做任何你想要的名字，只要确保名称是唯一的。然而，有一个问题：我们有没有办法引用这个collection，否则没有办法调用它。 

为了解决这个问题，我们需要一个变量与我们collection关联： 

PlayersList= new Meteor.Collection（'players'）; 

请注意，我们并没有使用var关键字来定义这个变量。这是因为我们希望有一个全局变量，因为这将使我们能够在整个应用中，而不是仅仅在其中的一个文件来引用变量。 

要确认collection存在，切换到谷歌Chrome浏览器并进入收集到JavaScript控制台的名称： 

PlayersList 

您应该看到以下内容：（见原文截图）


如果返回错误，这可能是因为你已经正确输入变量的名称，或犯了JavaScript文件中的语法错误。

如果没有返回错误，就可以继续进行。 

4.3插入数据 

我们的collection已经建立，但它是空的。为了解决这个问题，我们需要在collection中插入一些数据，来实现这一点，我们可以通过以下方法插入数据：

1，通过JavaScript控制台。 
2，通过JavaScript文件。 
3，通过在界面的form。 

我会在这本书中解释中这些方法，但是，正如你所看到的，通过操纵JavaScript控制台的数据库是最便捷的选择。 

首先，在JavaScript控制台输入以下代码： 

PlayersList.insert（）; 

这是我们用来调用数据库的语法。我们从collection中的变量开始。然后我们再加入函数。不过，虽然这是一个有效的MongoDB的声明，我们并没有通过框架向其传递任何数据，这种声明并没有做任何事情。该函数已经执行但collection保持不变。 

要真正将数据插入到collection，我们使用以下语法： 

PlayersList.insert({
    name: 'David',
    score: 0 
});

在这里，我们已经做了几件事情。

首先，我们在小括号中添加了一对大括号。我们的数据是结构化的JSON格式。如果你不熟悉的JSON格式，你会在学习的过程中了解到的。 （这并不困难。） 

其次，我们定义了我们的collection中fields的名字：

• name
• score

您可以命名这些fields任何你喜欢的名字，但是，因为我们将在代码中引用它们，最好让他们简单而直接。 

第三，我们要设置我们已经创建的fields的值。如表所示，我们给出的name的fields“David”的值和"score"的fields为“0”的数值：

            Filed                 Value

            name                  David
            score                 0

第四，我们已经用逗号来分隔不同的fields。这很容易忽视，但是，没有逗号我们会得到被反馈回错误。 （如果遇到错误，请检查逗号！）

最终，我们在这里所做的就是创建一个MongoDB的document-－ 相当于SQL术语中的row - 我们的计划是在我们的列表中创建每个球员的document。如果你想六名球员都在列表中，那你需要创建六个不同的document。

为了实现这一点，句法将是相同的。你只需要在JavaScript控制台中分别输入每个语句： 

PlayersList.insert({
    name: 'Bob',
    score: 0 
});


白色的空间是没有必要的，所以可以这样来写： 

PlayersList.insert({ name: 'Mary', score: 0 });

还要注意的是，每次插入后，数字和字母的混杂的一串乱码出现在JavaScript控制台。这乱码是MongoDB的每个document关联的唯一的ID。这个ID是primary key，在本教程中这将是非常重要的。 
 
在继续之前，只要你喜欢将尽可能多的球员输入你的collection。示例应用程序有六个球员，应该是足够了。

球员在我的名单如下：

• David 
• Bob
• Mary 
• Bill
• Warren 
• Tim

他们都为“0”分，在开始的时候球员们是否用不同的分数并不重要。 


4.4查找数据 

我们有一个包含我们的球员和他们的分数的collection，但我们希望能在需要的时候从collection调用数据。这可以通过查找功能来实现。 

在JavaScript控制台，输入以下命令： 

PlayersList.find（）; 

该声明将在collection中返回数据，但它不会呈现最可读的形式。这就是为什么用fetch功能将其配对列出：

PlayersList.find().fetch();

在JavaScript控制台工作时，读取功能由find函数返回到一个数组中的数据进行转换。只需点击向下箭头朝向看到包含在每个的docment中的数据。 

以从collection中检索一个精确选择的数据，而不是所有的数据，我们可以写 
成这样： 

PlayersList.find({ name: 'David' }).fetch();

在这里，我们通过查找功能传递一个fields和它的值，因此，我们能够只检索collection，球员的名字等于“David”的document。如果在collection中没有“David”命名的任何球员，将返回一个空数组。

还有一个有用的功能，也就是通过“find”函数的查找功能和“count”函数来计算collection中的符合条件的document数量的能力： 

PlayersList.find().count();

如果在collection中有六名球员（documents），这​​对一串函数将返回6。 

要从collection的数据中计算一个精确的数字，传递的东西通过
find函数和count函数一起实现，像这样：

PlayersList.find({ name: 'David' }).count();

在这里，我们计算collection中的documents的时候，其中的“name”这个fileds设置为“David”的数量。如果你的球员的名字之一被设定为“David”，这个语句将返回1，如果没有你的球员的名字设置为“David”，这条语句将返回0。 

在这一点上，我们只抓了查找功能的表面，但很多功能都可以通过我们学到的东西来获取。