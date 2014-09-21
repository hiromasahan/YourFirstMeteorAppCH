7 Sessions

在这个阶段，只要用户点击了名单li元素的项目函数就会执行。使用这个函数是因为我们要创建当players被点击选中时的效果。具体来说，当具有li元素的文本被点击时，click的event会改变它的背景颜色。我们将使用session实现这一目标。

session用来存储​​小块数据： 

•不保存到数据库中。 
•如果再次点击则不会被记录。 

这种临时数据可能现在并没用，但在创建界面时它是很方便的，并在Meteor自己的文档说明，session可以用来“在一个列表中当前选定的项目中存储相似的项目。”

7.1 超前思考

正如我刚才所说，在“Project”一章中： 

...在初学者对他们正试图建立什么有清晰的概念之前，让它们尝试写代码是一个常见的错误。 

这不仅适用于一个整体的项目。即使你有你正在试图建立项目的一个清晰的思路，这一切都太容易冲动，你会努力打造功能，而在此之前却没有完整的计划。 

对于小的功能，这不是什么大不了的事，因为你仍然可以用试错的进展像样的数目，但因为它会采取一些步骤来创建选择我们的球员名单上的效果，它有助于解释我们正在做的事情，而不只是直接一下到底。 

在某一时刻，我们将创建一个session，包含： 

•session的名字（称为键）。 
•session的值。 

该名称将是“selectedPlayer”，我们将在以后的时间使用这个名称来检索我们的session的值。session的值将被设置为在用户单击player的唯一ID。

如果你忘了“player的唯一ID”从何而来，可以回到“插入数据”一章。每一次我们在JavaScript控制台使用了插入功能，数字和字母混杂在一起出现的乱码就是唯一ID - primary key，如果你还记得我们刚刚创建的球员列表。这也是我们要存储到session中的ID。

给每个球员的唯一ID具有存取装置，则下面几条会按次序实现：

1 用户点击一个球员。 
2 click event触发。 
3 “selectedPlayer”session创建.这个session存储了我们刚刚建立的球员名称的ID。 
4，使用条件语句，判断储存在我们的sessionID是否和我们之前名单中的ID匹配。 
5，当匹配返回true，一个CSS样式被应用到该球员的li元素。这个类将改变li元素的背景颜色。

我在这里分享了更多比我通常会分享的概念，但理想情况下，你就会明白我们做什么之前，要知道我们要做什么。如果不是，那也没什么大不了的。你可能把下一章来运行过好几次（这也我一直建议的），但还会有很多实用性从这里开始了，这将帮助细节在脑海中更加深刻。 

7.2创建一个session 

在 click li.player 的event中，删除console.log语句，并插入以下行：

Session.set（'selectedPlayer“，”session value test'）; 

对于event的块中的代码现在应该类似于： 

Template.leaderboard.events（{
    'click li.player：function（）{
        Session.set（'selectedPlayer“，”session value test'）; 
    } 
}）; 

我们使用这里的语法像session的语法一样。正如我们之前所提到的，session只用两个数据即可定义： 

•session的名称。 
•session的值。 

在这个例子中，我们要存储到session中“session value test”这个字符串，我们已经把它命名为“selectedPlayer”。

为了证明我们的session是做什么的，让我们立即检索使用以下语法来获取session的值： 
Session.get（'selectedPlayer'）; 

在这里，我们使用get，而不是set，而我们只是想要得到session里传入到数据，向JavaScript控制台输出session的值，让我们来存放在变量中：

var selectedPlayer = Session.get（'selectedPlayer'）;

然后添加一个console.log语句如下：

console.log（selectedPlayer）; 

对于event块最终的代码应该类似于：


Template.leaderboard.events（{
    'click li.player：function（）{
        Session.set（'selectedPlayer“，”session value test'）;
        var selectedPlayer = Session.get（'selectedPlayer'）;
        console.log（selectedPlayer）; 
   }
}）; 

有了这个代码，当用户点击一个li元素的球员名称时，“session value test”字符串将被存储在一个session中，然后在同一个字符串会被立即输出到JavaScript控制台 

这不是很有用的代码，但它确实证明我们是如何能够存储一小部分数据的session中，然后进行检索。 
￼ 

7.3 player的ID

为了使我们的session变得有用，我们需要我们的session储存player到唯一ID。让我们在session.set之前创建一个playerId变量Session.set，语句如下：

var playerId=“session value test”;

然后更改Session.set语句中使用这个变量作为值的来源而非仅仅一个字符串，

Session.set（'selectedPlayer'，playerId）; 

event的函数中的代码现在应该类似于： 

var playerId =“session value test”; 
Session.set（'selectedPlayer'，playerId）; 
var selectedPlayer= Session.get（'selectedPlayer'）;
console.log（selectedPlayer）; 

但是，我们如何让这样的playerId变量的值被设置为用户点击球员名称的唯一的ID？代码本身是非常简洁的，但也有些奇怪，如下：

var playerId = this._id; 

困惑？ 
让我们来分析一下。 
首先，我们看一下this，this的值将依赖于上下文代码。在这种情况下，这指的是在用户点击了球员名称。

其次，_id部分是存储在player的唯一ID的字段的名称。在我们创建name和score的fields时，MongoDB以相同的方式创建_id的fields。 

（该字段被命名为_id，而不是“ID”，只是因为MongoDB就是这样命名fields的。我们已经创建字段和MongoDB中为我们创造字段之间是以下划线区分的。有它背后没有任何特殊含义。） 

通过将_id fields放在this后面，我们说，“给我的用户刚刚点击了球员的ID。”因此，当用户点击的球员名称时，playerId变量将存储在球员名称的ID中。 

至于我们的代码，这是它现在应该类似于： 

Template.leaderboard.events（{
   'click li.player：function（）{
    var playerId= this._id; 
    Session.set（'selectedPlayer'，playerId）; 
    var selectedPlayer= Session.get（'selectedPlayer'）;
    console.log（selectedPlayer）; 
  }
}）; 

另外值得一提的是，在单个session只能容纳一个数据块。因此，如果用户点击其他球员，该球员的唯一ID将覆盖以前存储在session中的ID值。 

￼ 

7.4  选择的效果 

当用户点击的球员之一时，具体地说，li的元素包含的球员之一，我们要把一个CSS类附加到li元素上。本课程将改变li元素的背景颜色。

要创建此功能，启动加入以下级的CSS文件： 

.selected{
    background-color：#CCC; 
} 

然后在JavaScript文件创建一个新的helper函数： 

Template.leaderboard.selectedClass = function（）{
    //code goes here
} 

我们已经附上这个helper的“leaderboard”template，并将给它分配一个“selected类”的名字。 

接下来，给这个函数添加一个return语句： 

Template.leaderboard.selectedClass=function（）{
    return‘select’; 
} 

然后切换到HTML文件，并更改这部分：

<li class =“player”>{{name}}{{score}}</li> 

......改成：

<li class =“player{{selectedClass}}”>{{name}}:{{score}}</li>

“leaderboard”的template现在应该类似于：

<template name =“leaderboard”> 
<ul> 
{{#each player}} 
<li class =“player{{selectedClass}}”>{{name}}{score}</li> 
{{/each}} 
</ul> 
</template> 

你可能会失望地发现，li元素现在有一个丑陋的灰色背景。如果你检查源代码，你会看到，这是因为我们已经附上了“select”类的li元素。这不是我们想要的确切结果，但它是朝着正确方向迈出的一步。 

下一步是在“selectedClass”中创建一个helper函数，来保存“selectedPlayer”session的值： 

var selectedPlayer= Session.get（'selectedPlayer'）;

这条语句后后，写出下面的语句：

var playerId = this._id; 

所以这和我们之前加入click li.player 的event的语句时一样的，并再次说明，我们使用它来检索一个球员的唯一ID和之前的是否相同。我们可以访问这个ID，因为{{selectedClass}} 标签在each模块中。其结果是，this指的是当前正在通过each循环迭代的部分，使我们能够获得player的_id的fields。这让我们可以在helper函数里面编写以下条件语句：

if（selectedPlayer=== playerId）{return ‘select’; 
} 

这里，我们比较存储在“selectedPlayer”session里已经通过each模块迭代过的球员的唯一ID。如果和之前的匹配起来，则返回并连接到相应的li元素的“选择”字符串。否则，没有任何反应。 

这是最后的辅助函数应该像这样： 

Template.leaderboard.selectedClass=function（）{
   var selectedPlayer= Session.get（'selectedPlayer'）; 
   var playerId= this._id; 
   if（selectedPlayer=== playerId）{
       return 'select'; 
  } 
} 
有了这个代码后，用户就可以单击包含一个任何li元素的球员的名字，发现由于我们的CSS文件中的定义，球员的背景色将会变化。这就创建当用户点击这些被选中的球员时候的效果。