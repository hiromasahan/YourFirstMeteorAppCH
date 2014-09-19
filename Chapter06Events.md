6 Events

目前，我们拥有的球员名单，但没有办法用这个球员名单进行交互。我们从collection中检索数据，但是，对于用户而言，Web应用程序是静态的。我们来解决这个问题，但首先，我们将创建在selector球员的效果。具体来说，当用户点击包含球员的名字的li元素上，li元素的背景会改变颜色。 
为了实现这一目标，将使用Meteor两个部分：

•events，来检测用户点击的东西。 
•session，存储他们已经点击了的数据。 

在本章中，我们将重点放在event。 


6.1创建event 

由Meteor创建event，我们可以在用户点击按钮，提交一个表单，按键盘上的按键，或其他很多事情的时候，我们能够追踪并回应触发相关代码。Meteor可以检测各种不同的event和我们能够让当这些event发生的事发生了。 

看到这个动作，isClient条件语句如下行： 

Template.leaderboard.events（）; 

这时，一些事情正在发生。 

首先，template通过关键字搜索应用程序可用的template。 （这正是我们创建的helper实现的功能。）

其次，leaderboard关键字是我们要与我们的event相关联的template的名称。与helper一样，event必须和一个template关联。

第三，我们使用event关键字来指定，接下来的代码块中，我们要创建一个或多个event。 

括号内语句结束的地方，我们将在JSON格式定义了我们的活动，这意味着我们将使用大括号：

Template.leaderboard.events({
    // events go here
});

然后，我们将使用以下花括号之间的语法创建我们的第一个event：

'click': function(){ 
    // code goes here
}

这个语句看起来有点怪异，但我会解释这是怎么回事。 

首先，我们定义的event类型。这是在引号内的“点击”部分。这意味着当用户单击某事，相关函数将被执行。我们还没有指出具体用户点击“leaderboard”template的周边的哪些的代码将被执行。

第二，我们已经给这个event一个函数，在这个函数中，我们可以写任何代码，我们希望（当点击时）event被触发执行。 

要看到这个动作，给event里面添加console.log语句： 

'click': function(){
    console.log("You clicked something");
}

整个event模块现在应该类似于：

Template.leaderboard.events({ 'click': function(){
        console.log("You clicked something");
    }
});

保存文件后，切换回谷歌浏览器，点击“leaderboard”template的范围内的任何地方。随着每一次点击，在“You clicked something”的字符串将出现在JavaScript控制台。 

随意（暂时）把click event改称dblClick event，因此event的函数，将响应双击，而不是单击动作： 

'dblclick': function(){
console.log("You double-clicked something");
}

然而，有一个很明显的问题： 

当用户单击我们的“leaderboard”template的任何地方，我们的代码都将会执行，而这还不是最有用的功能。我们希望一些特定的点击，像一个按钮或li元素被点击时，将会触发event的功能。 

为了实现这一目标，我们将使用event selector。 

6.2 event selector

通过使用event selector，我们可以将Web应用程序中的event加入的特定HTML元素。如果你曾经使用过jQuery，就会比较熟悉。如果没有，那么它也不会太复杂。

此前，我们把li的标签加入到HTML标签：

<li>{{name}}: {{score}}</li>

这是改变event——当我们点击这些li元素时才响应，而不是当我们点击任何地方都响应。下面是我们如何做到这一点： 

'click li': function(){
console.log("You clicked a list item");
}

在这里，我们做了两件事情： 

1 把li标签加入到click关键词后面。
2 改变了console.log的输出语句。

因为，首先，当点击li元素时，我们的event才会触发 - 与event的相关的函数代码才会执行。 

但是，如果我们的“leaderboard”的template，不是player的列表的一部分内有其他的li元素怎么办呢？你的假设是对的，这会导致其他问题。我们不希望event在我们不希望它触发时被触发。 

为了解决这个问题，我们可以添加一个player类，以我们的li元素：

<li class="player">{{name}}: {{score}}</li>

然后创建event时引用这个类：

'click li.player': function(){
console.log("You clicked a player's list item");
}

在这里，我们提出了类似之前的修改： 

1 把player类加入到li元素中。
2 改变了console.log的输出语句。

我们在定义event时有很大的灵活性。只要我们开始声明一个event类型，我们可以使用任何selector，因为我们通常会有CSS文件。为了让事情变得简单，在我们已经创建的event中坚持这一点。 


6.3 event类型 

在我们讨论Meteor时我尽可能的避免走弯路。我会尽快从A点讲到B点，然后帮助你有机会去实践代码。由于Meteor不同的event类型很有意思，我认为这值得考虑轻微地走些弯路。 

在Meteor中，有一系列可用的event类型： 

• click
• dblclick
• focus
• blur
• change
• mouseenter • mouseleave • mousedown • mouseup
• keydown
• keypress
• keyup
• submit

我们已经介绍了单击和双击，稍后会谈谈submitevent，但此时我的建议有： 

1，创建另一个Meteor项目。 
2，实践其他的event类型。 

你仅仅通过它们的名字可能不明白每个event类型的含义，但是此时谷歌搜索就会派上用场。 （搜索是软件开发过程中巨大的一部分，所以养成这种习惯——假设其他人也有过同样的问题） 

以下是对一些event类型的描述：

•当页面上的元素需要focus，例如，用户鼠标点击输入框时，表示focus event触发 
•当页面上不再聚焦，例如当用户删除他们的光标离开输入框，表示blur event触发。 
•当用户的光标悬停相关元素的边界内，表示MouseEnter event触发。 
•当用户的光标停止在关联元素的范围内徘徊，表示ouseLeave event触发。 
•当某些元素的值发生变化，比如当一个复选框被选中或未选中时发生时，change event触发。 
除了这些很有意思，用event类型试验的好处是，你会直观地知道什么时候使用它们会实现什么功能。这就是说，它是远远没必要纠结在这一点上，可以随意跳过这一部分。