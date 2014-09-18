5 Template

随着创建了一个collection和输入了一些数据，我们可以开始建立了Leaderboard的应用程序的用户界面。 
首先，安装我们的应用程序的基本HTML结构,先打开HTML文件，并粘贴以下代码进去：

<head> 
  <title>Leaderboard</title>
</head>
<body>
  <h1>Leaderboard</h1> 
</body>

似乎有几件事情没从这个代码显示出来： 

1 我们没包括html标签。 
2 我们并没有包括任何的JavaScript文件。 
3，我们并没有包括任何CSS文件。 

我们并没有包括这些事情，因为我们并不需要包括他们。Meteor会保存好它们。它会自动包含HTML标记，亦包括我们的应用需要的任何CSS和JavaScript文件中。 
一个Meteor的核心原则让开发者开心编程，所以有很多方便的功能，这样也许会让你的开发的生活更轻松。 

5.1 创建template

接下来，我们要创建一个template，并将户界面的部分放入template，我们可以使用JavaScript代码。 （我们将很快看到一个这样的例子。） 

要创建一个template，在HTML文件中添加下面的代码： 

<template name="leaderboard"> 
  Hello world
</template>

template有相当舒服的结构。我们使用这个template标签，并附上名称属性。我命名这个template为“Leaderboard”，但你可以用任何你喜欢的方式命名它。只知道，我们一会儿就会引用它，所以按照往常一样，这有助于选择的简单直接的名字，很容易记住。 
如果您将文件保存在当前状态下，template不会出现在网页浏览器中。这是我们原始的HTML代码。

怎么了？ 

好吧，template默认不显示，因为在某些情况下： 

•您可能需要一个template在特定的时间出现。 
•您可能需要一个template在特定的时间消失。 
•您可能需要一个template出现在多个位置。 

因此，template需要他们创建后手动加入操作。你可能会觉得这是一个额外的步骤，但随着深入的开发，它会变得越来越有用。 

为了让“Leaderboard”template显示在浏览器中，将body标签中加入：

{{>leaderboard}}

所以，很显然，这不是一个HTML标记。双花括号意味着它是Meteor的template的engine一部分 - Spacebars。通过Handlebars语法的启发，变成我们在HTML中使用的语句，让想要的东西的动态发生的语法。Spacebars将在本教程中频繁使用，但现在，只需要知道： 

1，所有Spacebars标签使用双大括号来和其他区分开来。 2，当我们想用一个template的时候，在括号中用大于号。

在这个阶段中，HTML文件应该类似于： 

<head> 
  <title>Leaderboard</title>
</head>
<body>
  <h1>Leaderboard</h1>
  {{> leaderboard}}
</body>

<template name="leaderboard"> 
  Hello World
</template>

保存文件后，“Hello World”文本应该出现在浏览器中。 


5.2创建一个helper

在我们的HTML文件中，我们有一个template，但它是静态的。它除了显示什么无聊的“Hello World”文本什么都不会。为了解决这个问题，我们可以创建一个helper，helper是附着于template的JavaScript函数。这是对代码最好的理解，让我们试一下。

切换到JavaScript文件，输入下面的语句：

Template.leaderboard.player 

这是我们用它来创建一个helper的语法，它有三个部分。

首先，我们使用template的关键字，通过Meteor应用对template进行搜索。此时只有一个template，但一个完整的应用程序可能有几十个甚至上百个。

其次，我们采用了leaderboard关键字来引用我们先前创建的template的名称。我们创建的每一个helper函数需要被连接到唯一的template。

第三，我们使用player关键字来命名我们的helper。我们将引用这个名称在HTML文件中，并再次说明一下，你可以用任何你喜欢的名字。

在这个helper下写代码，我们把它和一个函数像这样联系起来： 

Template.leaderboard.player = function(){ 
     // code goes here
}

我们没有在这里做什么特别的事情。这只是一个普通的函数。我们在html文件引用球员名称时候这个函数内部的代码将会执行。 

然而，有一个问题。如果你保存文件，然后切换回谷歌Chrome浏览器，你会看到该应用程序被崩溃了。这是因为我们没有考虑到一个事实，即一些代码不能在客户端和服务器上同时运行。 

由于我们正在与template打交道，你知道，用户界面的东西 - helper在server端没有任何意义显示出来。其结果是，Meteor不让它在服务器上运行和并返回代码错误的指示。 

我们所要做的就是把helper函数放在isClient的条件语句中：

if(Meteor.isClient){
  // put the helper here
}

代码应当变成

if(Meteor.isClient){ 
   Template.leaderboard.player = function(){ 
   // code goes here
   }
}


保存文件后，错误就会消失。 

终于让我们的helper函数的一些功能，加一个return语句在里面： 

Template.leaderboard.player = function(){ 
   return "A list of players should be here.";
}

然后切换回HTML文件，去掉“Hello World”文本，然后将下面的标记的“Leaderboard”template中： 

{{player}}

在这里，我们使用的是另一个Spacebars标签（通过这双大括号得知）。但这里我们不使用大于号。这是因为我们并不是建立template。我们是引用我们的helper函数的名称的实现，所以只用双大括号。

保存文件后，return语句的文本将显示在浏览器中。这不是一个大的能令人兴奋的功能，但是： 

1，这类似于我们一会儿将如何使用helper，所以Meteor的一个特点是简单，而不是我想让它很简单。 
2，我们要利用helper来检索我们收集了球员，并界面中显示出来，这就有趣多了。 

让我们继续。 


5.3 each Blocks
在“player”helper中，改变return语句如下： 

Template.leaderboard.player = function(){ 
  return PlayersList.find();
}

在这里，我们使用了我们前面所提到的查找功能，像以前一样，我们将其连接到保存球员名单的collection。因为我们把这个语句放到helper里，可以从这个template的函数中找到，并显示它的检索到的数据。 

要做到这一点，切换到HTML文件，并删除此标签： 

{{player}}

这个标签确实能指向helper，但不是我们想要的方式。相反，我们想要写在我们的“Leaderboard”template中的以下内容：

{{#each player}}
test
{{/each}}

在这里，需要解释以下几件事情是怎么回事。

首先，球员列表在引用中被“player”函数检索。 
其次，我们遍历这些球员的each block（再次使用Spacebars语法）。 
第三，我们为每个在列表中的球员输出单词“test”。

从概念上讲，它就像我们有一个数组： 

var player = ['David', 'Bob', 'Bill', 'Mary', 'Warren', 'Tim'];

......它就像我们使用forEach循环来遍历这个数组中的值：

player.forEach(function(){ 
   document.write('test');
});

其结果是，单词“test”出现的次数等于player的集合中的数量。如果有六名球员集合中，单词“test”会出现六次。 

在已经创造的这个each block中，我们也可以检索与数据结合的fields。我们提取从“PlayersList”收集的数据，所以我们可以呈现name和score的filed显示的两个值。

为了使我们的名单显示球员的名称列表，例如，我们可以这样写： 

{{#each player}}
    {{name}}
{{/each}}

（当引用fields的时候，我们只用大括号括住fields的名称。） 

我们还可以在球员的名字旁边显示它们的分数： 

{{#each player}}
    {{name}} {{score}}
{{/each}} 

不过，虽然我不是太关心使用户界面是否美观- 我总是发现，专注于设计技术的书是浪费时间 - 但界面结构稍微好一点也是有益的：

<ul>
   {{#each player}}
       <li>{{name}}: {{score}}</li>
   {{/each}} 
</ul>

球员的名字将出现在他们的名字旁边的分数列表。默认情况下，玩家将通过输入进collection的时间升序顺序（从前往后）进行排序。 
 
如果你曾经用过Meteor以外的架构或语言，你也许可以看到，我们用少量的代码得到了较多的功能，它会从此变得更好。