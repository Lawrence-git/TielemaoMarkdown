JavaScript-猜数字
===

[[toc]]

# 进入JavaScript的第一课


现在我们来学习一些关于JavaScript的理论，以及您可以拿它来做的事情。通过一个完整的练习，我们将会给您一个了解JavaScript基本功能的速成课。您将一步一步地建立一个简单的“猜数字”游戏。

您不会被要求立即理解所有代码的细节——现在我们只是想把高级概念介绍给您，以及给您一个JavaScript（以及其他编程语言）是如何工作的。在随后的文章中，您将会再次看到这些功能的更多细节。

Note: 您在JavaScript中看到的许多代码特性和其他编程语言类似— 函数、循环，等等。 代码语法看起来不同，但是在概念上是基本类似的。

## 像程序员一样思考

在编程中学习的最困难的事情之一不是我们需要学习的语法，而是如何应用它来解决现实世界的问题。 您需要像一个程序员一样开始思考——这通常涉及到对程序需要做什么样描述，以及实现这些东西需要什么代码特性，以及如何使它们一起工作。

这需要努力工作，编程语法的经验和实践的混合，以及一点创造力。 我们编写的代码越多，我们的本领就越好。 我们不能保证您将在5分钟内开发“程序员大脑”，但我们将给您很多机会像整个课程中的程序员一样练习思维。

考虑到这一点，让我们看看我们将在本文中构建的示例，并查看将其分解为有形任务的一般过程。

## 例子——猜数字游戏

在本文中，我们将向您演示如何构建您在下面看到的简单游戏：

![js-GuessNumber]($res/js-GuessNumber.gif)

不妨设想，老板给了以下要求并让您设计一个游戏：

> 我想让你创建一个可以猜数字的游戏，它会在1~100以内随机选择一个数, 然后让玩家挑战在10轮以内猜出这个数字，每一轮都要告诉玩家正确或者错误， 如果出错了，则告诉他数字是低了还是高了，并且还要告诉玩家之前猜的数字是什么。 一旦玩家猜测正确，或者他们用完了回合，游戏将结束。 游戏结束后，可以让玩家选择再次开始。

看到这个要求，我们可以做的第一件事是开始把它分解成简单的可操作的任务，尽可能从程序员的思维去思考：

1.  生成1到100之间的随机数。
2.  记录玩家在第几轮。从1开始。
3.  为玩家提供一种猜测数字的方法。
4.  一旦提交了猜测，首先将它记录在某处，以便用户可以看到他们先前的猜测。
5.  接下来检查它是否是正确的数字。
6.  如果是正确的：
    1.  显示祝贺消息。
    2.  阻止玩家输入更多的猜测（这会使游戏混乱）。
    3.  显示控制允许玩家重新开始游戏。
7.  如果它错了，并且玩家有剩余轮次：
    1.  告诉玩家他们错了。
    2.  允许他们输入另一个猜测。
    3.  将圈数增加1。
8.  如果它是错误的，并且玩家没有剩余轮次：
    1.  告诉玩家游戏结束。
    2.  阻止玩家输入更多的猜测（这会使游戏混乱）。
    3.  显示控制允许玩家重新开始游戏。
9.  一旦游戏重新启动，请确保游戏逻辑和用户界面完全重置，然后返回步骤1。

让我们继续，看看我们如何将这些步骤转换为代码，构建示例，并探索JavaScript的功能。

### 初始设置

开始本教程前，我们希望您能拷贝一份本地副本 [number-guessing-game-start.html](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/first-splash/number-guessing-game-start.html)    ([see it live here](http://mdn.github.io/learning-area/javascript/introduction-to-js-1/first-splash/number-guessing-game-start.html))。在文本编辑器和Web浏览器中打开它，此时，您将看到一个简单的标题，用于输入猜测的说明和形式段，但表单目前不会执行任何操作。

我们将添加的所有代码放在HTML底部的 [<script>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script) 元素中：

```html
<script>

  // Your JavaScript goes here

</script>
```

### 添加变量以保存数据

让我们开始吧。 首先，在 [<script>](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/script)元素中添加以下行：

```js
var randomNumber = Math.floor(Math.random() * 100) + 1;

var guesses = document.querySelector('.guesses');
var lastResult = document.querySelector('.lastResult');
var lowOrHi = document.querySelector('.lowOrHi');

var guessSubmit = document.querySelector('.guessSubmit');
var guessField = document.querySelector('.guessField');

var guessCount = 1;
var resetButton;
```

这部分代码设置了存储我们的程序将使用的数据的变量。 变量基本上是值（例如数字或文本字符串）的容器。 您可以使用关键字`var`以及变量的名称创建一个变量。 然后，您可以使用等号（=）和您要赋予的值为变量赋值。

在我们的示例中：

*   第一个变量 - randomNumber - 被分配一个1到100之间的随机数，使用数学算法计算。
*   接下来的三个变量都用于存储对HTML中的结果段落的引用，并用于在代码的后面段落中插入值： 
*   ```html
    <p class="guesses"></p>
    <p class="lastResult"></p>
    <p class="lowOrHi"></p>
    ```

*   接下来的两个变量存储对表单文本输入和提交按钮的引用，并用于控制以后提交猜测：

    ```html
    <label for="guessField">Enter a guess: </label><input type="text" id="guessField" class="guessField">
    <input type="submit" value="Submit guess" class="guessSubmit">
    ```

*   我们的最后两个变量存储一个猜测计数1（用于跟踪玩家有多少猜测），以及一个不存在的引用（但稍后会有）。

**Note**: 在稍后的课程中，您将学到更多关于变量的信息。[查看下一篇文章](https://developer.mozilla.org/en-US/docs/user:chrisdavidmills/variables)。

### 函数（Function）

接下来，在您之前写入的JavaScript代码段中添加以下内容：

```js
function checkGuess() {
  alert('I am a placeholder');
}
```

函数是可重复使用的代码块，您可以“编写一次，到处运行”，从而节省了大量的重复代码， 这真的很有用。 有许多方法来定义函数，但现在我们先将注意力集中在当前这个简单的方式上。 这里我们使用关键字`function`定义了一个函数，后面跟着一个名字，再后面加了括号。 之后，我们放两个大括号（`{}`）。 在大括号里面，放置着当我们调用该函数时所有想要运行的代码。

该代码通过键入函数的名称后跟括号运行。

请尝试立即保存您的代码，然后在浏览器中刷新。

进入  [developer tools JavaScript console](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), 并输入以下代码：

```js
checkGuess();
```

![js-console]($res/js-console.gif)


您应该看到了一个告警窗口，“I am a placeholder”，我们在代码中定义了一个函数，当我们调用它时，函数创建了一个告警窗口。

**Note**: 您将在后续课程中学到更多有关函数的知识。

### 运算符（Operators）

JavaScript运算符允许我们执行比较，做数学运算，连接字符串，以及其他类似的事情。

让我们保存代码并刷新浏览器中显示的页面。 如果您还没有打开 [developer tools JavaScript console](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools) ，或者说您无法访问浏览器开发人员工具，您可以使用使用下面所示的简单内置控制台——您可以尝试键入下面所示的示例代码，并在每个命令输入完毕之后按下Return / Enter，查看他们执行后的返回结果。 

附：简单内置控制台代码
```html
<!-- Learn about this code on MDN: https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/First_steps/A_first_splash -->

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>JavaScript console</title>
    <style>
      * {
        box-sizing: border-box;
      }

      html {
        background-color: #0C323D;
        color: #809089;
        font-family: monospace;
      }

      body {
        max-width: 700px;
      }

      p {
        margin: 0;
        width: 1%;
        padding: 0 1%;
        font-size: 16px;
        line-height: 1.5;
        float: left;
      }

      .input p {
        margin-right: 1%;
      }

      .output p {
        width: 100%;
      }

      .input input {
        width: 96%;
        float: left;
        border: none;
        font-size: 16px;
        line-height: 1.5;
        font-family: monospace;
        padding: 0;
        background: #0C323D;
        color: #809089;
      }

      div {
        clear: both;
      }

    </style>
  </head>
  <body>

    
  </body>

  <script>
    var geval = eval;

    function createInput() {
      var inputDiv = document.createElement('div');
      var inputPara = document.createElement('p');
      var inputForm = document.createElement('input');

      inputDiv.setAttribute('class','input');
      inputPara.textContent = '>';
      inputDiv.appendChild(inputPara);
      inputDiv.appendChild(inputForm);
      document.body.appendChild(inputDiv);
      inputDiv.focus();

      inputForm.addEventListener('change', executeCode);
    }

    function executeCode(e) {
      try {
        var result = geval(e.target.value);
      } catch(e) {
        var result = 'error — ' + e.message;
      }

      var outputDiv = document.createElement('div');
      var outputPara = document.createElement('p');

      outputDiv.setAttribute('class','output');
      outputPara.textContent = 'Result: ' + result;
      outputDiv.appendChild(outputPara);
      document.body.appendChild(outputDiv);

      e.target.disabled = true;
      e.target.parentNode.style.opacity = '0.5';

      createInput()
    }
    
    createInput();

  </script>
</html>
```
效果，可以直接在简易控制台（其实也就是一个html网页）上进行运算：

![javaScript-console]($res/javaScript-console.gif)

首先让我们来看看算术运算符，例如：

| 运算符 | 名称 | 示例 |
| --- | --- | --- |
| `+` | 加法运算符 | `6 + 9` |
| `-` | 减法运算符 | `20 - 15` |
| `*` | 乘法运算符 | `3 * 7` |
| `/` | 除法运算符 | `10 / 5` |

您也可以使用`+`运算符将文本字符串连接在一起（在编程中这称为_连接_）。 尝试输入以下行：

```js
var name = 'Bingo';
name;
var hello = ' says hello!';
hello;
var greeting = name + hello;
greeting;
```

还有一些快捷操作符可用，称为_增强赋值操作符_。 例如，如果您想简单地添加一个新的文本字符串到一个现有的并返回结果，您可以这样做：

```js
name += ' says hello!';
```

这相当于：

```js
name = name + ' says hello!';
```

当我们执行true / false比较时（例如在条件语句 - 见下面），我们使用比较运算符，例如：

| 运算符 | 名称 | 示例 |
| --- | --- | --- |
| `===` | 严格等运算符（它们是否**确实**一样？） | `5 === 2 + 4` |
| --- | --- | --- |
| `!==` |  严格不等运算符（它们不一样么？） | `'Chris' !== 'Ch' + 'ris'` |
| --- | --- | --- |
| `<` | 小于运算符 | `10 < 6` |
| --- | --- | --- |
| `>` | 大于运算符 | `10 > 20` |
| --- | --- | --- |

### 条件（Conditionals）

回到我们的`checkGuess()`函数，现如今，我们不希望它只是吐出一个占位符消息；我们希望它能够检查玩家的猜测是否正确，并做出适当的反应。

现在，将您当前的`checkGuess()`函数替换为此版本：

```js
function checkGuess() {
  var userGuess = Number(guessField.value);
  if (guessCount === 1) {
    guesses.textContent = 'Previous guesses: ';
  }
  guesses.textContent += userGuess + ' ';

  if (userGuess === randomNumber) {
    lastResult.textContent = 'Congratulations! You got it right!';
    lastResult.style.backgroundColor = 'green';
    lowOrHi.textContent = '';
    setGameOver();
  } else if (guessCount === 10) {
    lastResult.textContent = '!!!GAME OVER!!!';
    setGameOver();
  } else {
    lastResult.textContent = 'Wrong!';
    lastResult.style.backgroundColor = 'red';
    if(userGuess < randomNumber) {
      lowOrHi.textContent = 'Last guess was too low!';
    } else if(userGuess > randomNumber) {
      lowOrHi.textContent = 'Last guess was too high!';
    }
  }

  guessCount++;
  guessField.value = '';
  guessField.focus();
}
```

这有很多代码，让我们给您逐段解释它的意图。

*   第一行（上面的第2行）声明了一个名为`userGuess`的变量，并将其值设置为在文本字段中输入的当前值。 我们还通过内置的`Number()`方法运行这个值，只是为了确保该值绝对是一个数字。
*   接下来，我们遇到我们的第一个条件代码块（上面的3 - 5行）。 条件代码块允许您根据某个条件是否为真从而选择性地运行代码。 它看起来有点像一个函数，但它不是。 条件块的最简单形式是从关键字`if`开始，然后是一些括号，然后是一些花括号。 在括号内，我们包含了一个比较。 如果比较返回true，就会执行放在花括号中的代码。 反之，花括号中的代码就会被跳过，从而执行下一行代码。 在这种情况下，比较是测试`guessCount`变量是否等于1，即玩家是不是第一次猜数字：

*   ```js
    guessCount === 1
    ```

    如果是, 我们让`guesses`段落的文本内容等于"Previous guesses: "。如果不是，那就说明我们已经执行过了文本设定，那就无需再次设定了。
*   第6行将当前`userGuess`值附加到`guesses`段落的末尾，并加上一个空格，因此在每个猜测值之间将有一个空格。
*   下一个代码块中（上面的第8-24行）做了几个检查：
    *   第一个`if(){ }`检查用户的猜测是否等于在代码顶端设置的`randomNumber`值。如果是，则玩家猜对了，游戏胜利，我们将向玩家显示一个漂亮的绿色的祝贺信息，并清除猜测信息框的内容，调用`setGameOver()`方法。
    *   紧接着我们又写了一个`else if(){ }` 结构。它会检查这个回合是否是玩家的最后一个回合。如果是，程序将做与前一个程序块相同的事情，除了它显示的是Game Over而不是祝贺消息。
    *   最后的一个块是`else { }`，其中包含着前面两个比较都为false才会执行的代码，即玩家还有游戏的次数但是本次没猜对。在这个情况下，我们会告诉玩家他们猜错了，并执行一个条件测试，判断并告诉玩家猜测的数字是大是小。
*   这个函数最后三行 （上面的第26–28行） 让我们为下次猜测值提交做好准备。我们把`guessCount`变量的值+1，因此玩家消耗了一次机会 （`++`是一个递增操作符 - 将值+1），然后我们把文本段的值清空，重新将焦点设置在文本段里，准备下一轮游戏。

### 事件（Events）

现在，我们有一个实现比较不错的`checkGuess()`函数了，但它现在不会做任何事情，因为我们还没有调用它。 理想情况是，我们希望在按下“Submit guess”按钮时调用它，为此，我们需要使用事件。 事件是浏览器中发生的动作，例如点击按钮，加载页面或播放视频，我们可以调用代码来响应。 侦听事件发生的构造方法称为**事件监听器**，响应事件触发而运行的代码块被称为**事件处理器**。

在`checkGuess()`函数的结束大括号后添加以下代码：

```js
guessSubmit.addEventListener('click', checkGuess);
```

这里我们为`guessSubmit`按钮添加了一个监听事件。这个方法包含两个可输入值（参数），监听事件的类型（在本例中为“点击”），和当事件发生时我们想要执行的代码（在本例中为`checkGuess()`函数）——注意，当函数作为事件监听方法的参数时，函数名后不应带括号。

保存您的代码并刷新页面，示例某些功能现在应该能正常工作了。 现在唯一的问题是，如果你猜到正确的答案或游戏次数已使用完，游戏将发生错乱，因为我们还没有定义应该在游戏结束后运行的`setGameOver()`函数。 现在，让我们添加缺少的代码，并完成示例功能。

### 完善游戏

让我们将`setGameOver()`函数添加到代码底部，然后再来看看它：

```js
function setGameOver() {
  guessField.disabled = true;
  guessSubmit.disabled = true;
  resetButton = document.createElement('button');
  resetButton.textContent = 'Start new game';
  document.body.appendChild(resetButton);
  resetButton.addEventListener('click', resetGame);
}
```

*   前两行禁用表单文本输入和按钮，方法是将其`disable`属性设置为true。 这是有必要的，如果我们没有禁用，用户可以在游戏结束后提交更多的猜测，这会破坏游戏的规则。
*   接下来的三行创建了一个新的[button](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button)元素，设置它的文本为“Start new game”，并把它添加到我们文档的底部。
*   最后一行在我们的新按钮上设置了一个事件监听器，当它被点击时，一个名为`resetGame()`的函数被将被调用。

现在我们需要定义`resetGame()`这个函数了！ 将以下代码添加到代码底部：

```js
function resetGame() {
  guessCount = 1;

  var resetParas = document.querySelectorAll('.resultParas p');
  for (var i = 0 ; i < resetParas.length ; i++) {
    resetParas[i].textContent = '';
  }

  resetButton.parentNode.removeChild(resetButton);

  guessField.disabled = false;
  guessSubmit.disabled = false;
  guessField.value = '';
  guessField.focus();

  lastResult.style.backgroundColor = 'white';

  randomNumber = Math.floor(Math.random() * 100) + 1;
}
```

这个相当长的代码块完全重置了一切：

*   将`guessCount`重置为1。
*   清除所有信息段落。
*   从我们的代码中删除重置按钮。
*   启用表单元素，并清空和聚焦文本字段，准备接受用户输入的新猜测。
*   从`lastResult`段中删除背景颜色。
*   生成一个新的随机数，这样下次您就是在猜测新的数字了！

**现在，您应该有一个能完整工作的（简单）游戏了——恭喜您 。**

我们现在来讨论下其他很重要的代码功能，你可能已经看到过，但是你可能没有意识到这一点。

### 循环（Loops）

上面代码的一部分，我们需要更详细地看一下[ for](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) 循环。 循环在编程中是一个非常重要的概念，它允许你一直重复运行一段代码，直到满足某个条件。

首先，请再次转到 [浏览器开发工具 JavaScript 控制台](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)然后输入以下内容：

```js
for (var i = 1 ; i < 21 ; i++) { console.log(i) }
```

发生了什么？ 数字1到20在控制台中打印出来。 这是因为循环。 for循环需要三个输入值（参数）：

1.  起始值：在这种情况下，我们开始计数为1，但这可以是任何你喜欢的数字。 你可以用任何你喜欢的名字替换我，但我用作一个约定，因为它很短，很容易记住。
2.  退出条件：这里我们指定`i`小于21 - 循环将继续，直到`i`不再小于21。当`i`等于21时，循环不再运行 。
3.  增量：我们定义了`i++`，意思是向`i`加1。`i`值的每次变动都会引发循环的执行，直到i值等于21（就像上边讨论的那样）。在本例中，我们通过`console.log()`方法简单地在控制台打印出每次迭代时变量i的值。

现在让我们看看我们的猜测游戏循环 - 以下可以在`resetGame()`函数中找到：

```js
var resetParas = document.querySelectorAll('.resultParas p');
for (var i = 0 ; i < resetParas.length ; i++) {
  resetParas[i].textContent = '';
}
```

此代码在<`div class="resultParas">`内使用`querySelectorAll()`方法创建一个变量包含一个列表中的所有段落，然后依次通过每个段落，删除每个段落的文本内容。

### 一些关于函数的事情

让我们再来一次最后的改进，然后再讨论。 在`var resetButton`下面添加以下行，行靠近JavaScript的顶部，然后保存您的文件：

```js
guessField.focus();
```

这一行使用[`focus()`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/focus "HTMLElement.focus()方法可以设置指定元素获取焦点。") 方法立即自动地放置文本光标在输入框内，这意味着当页面加载完成时，用户可以马上开始他们的第一次游戏，而不需要去点击输入框。 这只是一个小的改进，但它提高了可用性 —— 给用户提供了可视化的线索去告诉他们该怎么开始这个游戏。

让我们分析一下在这里有更多的细节。在JavaScript中，一切都是对象。对象是存储在单个分组中的相关功能的集合。您可以创建自己的对象，但这是相当先进的，我们现在不会谈及它，直到很晚以后的课程。现在，我们将简要讨论您的浏览器包含的内置对象，它允许您做许多有用的事情。

在这种特殊情况下，我们首先创建了一个`guessField`变量，用于存储对HTML中的文本输入表单字段的引用 - 在顶部附近的变量声明中可以找到以下行：

```js
var guessField = document.querySelector('.guessField');
```

我们使用了`document`对象的`querySelector()`方法来获得此引用。`querySelector()`需要一个参数 — — 用该元素的 CSS 选择器选择您想要的引用的元素。

因为`guessField`现在包含对`<input>`的元素的引用，它现在将访问数量的属性 （存储于内部对象的其中一些不会更改其值的基础变量） 和方法 （存储在对象内部的基础函数）。一种方法可用来输入元素是`focus()`，所以我们现在可以使用这条线集中文本输入︰

```js
guessField.focus();
```

不包含对表单元素引用的变量不会有`focus()`方法供它们执行。例如，包含对`<p>`元素的引用的`guesses`变量和包含了一个数字的`guessCount`变量。

### 操作浏览器对象

让我们来稍微的使用一些浏览器对象。

1.  首先在浏览器中打开你的程序。
2.  接下来打开浏览器上的[开发者工具](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)， 并且切换到JavaScript 控制台的标签页。
3.  输入 `guessField` 后控制台将会显示一个包含[`<input>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input "HTML <input> 元素用于为基于Web的表单创建交互式控件，以便接受来自用户的数据。") 元素的变量。你会注意到控制台能够自动补全执行环境中存在的对象的对象名，包括你的变量名！
4.  现在输入下面的代码：

    ```js
    guessField.value = 'Hello';
    ```

    `value` 属性表示当前文本域中的值。 你能够看到输入这条指令后文本域中的值被我们改变了！
5.  现在试试输入 `guesses` 然后按下回车键。控制台会显示一个包含 [`<p>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/p "HTML <p>元素（或者说 HTML 段落元素）表示文本的一个段落。该元素通常表现为一整块与相邻文本分离的文本，或以垂直的空白隔离或以首行缩进。另外，<p> 是块级元素。") 元素的变量。
6.  现在试试输入下面这一行：

    ```js
    guesses.value
    ```

    浏览器会返回 `undefined`，因为段落中并不存在 `value`。
7.  为了改变段落中的文本内容， 你需要用 [`textContent`](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/textContent "Node.textContent 属性表示一个节点及其后代的文本内容。") 属性来代替 `value`。试试这个：

    ```js
    guesses.textContent = 'Where is my paragraph?';
    ```

8.  现在来一些有趣的东西。 试试一个接一个的输入下面这几行：

    ```js
    guesses.style.backgroundColor = 'yellow';
    guesses.style.fontSize = '200%';
    guesses.style.padding = '10px';
    guesses.style.boxShadow = '3px 3px 6px black';
    ```

9.  页面上的每个元素都有一个style属性，它本身包含一个对象，其属性包含应用于该元素的所有内联CSS样式。 这允许我们使用JavaScript在元素上动态设置新的CSS样式。

### 完成了...

所以这就是我们建立的这个例子— 已经结束了，做得好！ 尝试运行您的最终代码，或者[看看我们的版本](http://mdn.github.io/learning-area/javascript/introduction-to-js-1/first-splash/number-guessing-game.html)。如果您不能让示例工作，请检查 [source code](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/first-splash/number-guessing-game-start.html)。

