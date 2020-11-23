# 现代化最佳实践
章节目录：
- [最佳实践：JavaScript框架](#最佳实践JavaScript框架)
- [最佳实践：包管理和模块打包](#最佳实践包管理和模块打包)
- [最佳实践：CSS-in-JS](#最佳实践CSS-in-JS)
- [最佳实践：单页应用](#最佳实践单页应用)

*（以及为什么它们是糟糕的）*

如果要我用一句话来总结现代 web 开发，那就是，与 JavaScript 有关的一切。

现代 web 开发的很多东西都是围绕着 JavaScript 建立的。
## 最佳实践：JavaScript框架
现代Web开发最佳实践的核心是 Angular、React 和 Vue 等框架。

它们几乎列在我看到的每一个前端开发人员的工作描述上。“你用的是什么框架？”，当有人在 Twitter 上宣布一个很棒的新项目时，这是一个很常见的问题。

### 这些工具有什么吸引力？
框架的一大魅力在于它们如何创建用户界面。比方说，你想建立一个 todo list 应用，你有一些关于用户和他们的 todo 项目的数据。

```js
var data = {
    todos: [
        {
            item: 'Adopt a puppy',
            completed: true
        },
        {
            item: 'Buy dog food and a dog bed',
            completed: false
        }
    ],
    username: 'Chris'
};
```

您希望用户界面看起来像这样，有一个添加待办事项的字段，以及一个待办事项的列表。

已完成的项目应该被选中并划线。

```html
<h1>Chris's Todos</h1>

<form id="add-todo">
	<label for="todo-field">What do you want to get done today?</label>
	<input type="text" id="todo-field">
	<button>Add Todo</button>
</form>

<ul>
	<li class="completed">
		<label>
			<input type="checkbox" checked>
			Adopt a puppy
		</label>
	</li>
	<li>
		<label>
			<input type="checkbox">
			Buy dog food and a dog bed
		</label>
	</li>
</ul>
```

每当他们添加一个新的项目，或者完成一个项目，你就需要更新 UI。

如果是传统的 DOM 操作，那就需要在UI中获取最后一个项目，创建一个新的项目，然后添加到其中。或者当一个项目被勾选后，添加一个类。

而如果用户没有 todo 项目，也许你想显示一条消息，要求他们添加一个项目，这就需要你在他们添加第一个项目时，针对UI进行不同的操作。

```html
<h1>Chris's Todos</h1>

<form id="add-todo">
	<label for="todo-field">What do you want to get done today?</label>
	<input type="text" id="todo-field">
	<button>Add Todo</button>
</form>

<p>You don't have any todos yet. Please add one.</p>
```

### 基于状态的 UI
框架使用了一种叫做基于状态或基于数据的 UI 的方法。

你为一个 UI 创建一个模板，然后告诉框架，“如果我的数据看起来像这样，就做这个。如果它看起来像那样，就做别的事情。”

```js
if (data.todos.length > 0) {
    // create list items
} else {
    return 'You don\'t have any todos yet. Please add one.';
}
```

你不需要直接更新 UI，而是更新你的数据，剩下的事情由框架来处理。它通过一个叫做 diffing 的过程来寻找当前 UI 和它应当呈现样子之间的差异，并且只更新需要更新的东西。

它真的很聪明。

### 规模化的性能
大名鼎鼎的框架（ React、Vue 等）都使用了一种叫做虚拟 DOM 的东西。这是一个基于 JS 的真实 UI 的表示，当数据发生变化时，它用来计算出 UI 中需要更新的内容。

当处理真正的大型数据集时，它的性能可以比尝试查询实际的 DOM 更好。对于需要“大规模”执行的应用来说，这是框架经常被提及的一个好处。

### 更少的漏洞
而且由于成熟的框架被数百或数千家公司的数千名开发人员使用，你有更多的人测试代码，发现错误，并推送修复。

代码会更有弹性。

### 那么，问题在哪里？
你可能会想：这都是有道理的，听起来也不错。这有什么问题呢？

使用框架的主要论点之一是性能，但框架也会造成很多性能问题。

我们经常谈论规模化的性能。

但这些工具是由那些需要处理规模化的公司设计的，而我们大多数人不会。我们构建的大多数网站都没有 Facebook 或 Twitter 大小的数据集，大多数网站也从来没有达到这个水平。

我们喜欢框架有大量的 bug修复，但其中很多是针对我们永远不会遇到的奇怪边缘情况和我们不需要的功能的修复。

我们继承的是别人的问题的解决方案，而不是我们的。

而且我们为了解决规模问题和边缘情况下的 bug 而加载的所有 JS 对性能有很大的影响，特别是首页加载。

我们牺牲了初始页面的加载性能，来换取以后更快的页面加载速度，而初始页面是用户对我们网站或应用的第一印象。

我不太确定这种权衡是否值得。
### JavaScript是技术栈中最昂贵的部分
由于浏览器的工作方式，JavaScript 的性能比 HTML 和 CSS 差很多。

Addy Osmani 写了一篇很棒的[文章](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4)来说明为什么会这样，他指出：

> 逐个字节来看，JavaScript 仍然是我们发送到手机上的最昂贵的资源，因为它可以在很大程度上延迟交互性。
>
> 【原内容】：Byte-for-byte, JavaScript is still the most expensive resource we send to mobile phones, because it can delay interactivity in large ways.

几年前有一个活动叫 “少用1个 JPG”。

其论点是，与其担心200-300kb的 JS，不如少用一个 JPG，它们大小差不多。

问题是，这不仅仅是下载大小的问题。

JS 对解析和执行的要求更高，它阻止了页面的渲染。它阻止了其他文件的下载，它只有在浏览器接收到它之后才会运行。

在同一篇文章中，Addy Osmani 写道：

> 它需要被解析、编译、执行，还有许多其他的步骤是引擎需要完成的。
>
>【原内容】：[It needs to be] parsed, compiled, executed —and there are a number of other steps that an engine needs to complete.

所有这些 JavaScript 对于前端性能来说都是毁灭性的。

2019年9月，Zach Leatherman 在[推特](https://twitter.com/zachleat/status/1169998370041208832)上写道：

>哪个有更好的首次有效绘制时间？
>- 一个8.5MB的原始 HTML 文件，其中包含了我27506条推文中每一条推文的全文
>- 一个仅有一条推文的客户端渲染的 React 网站
>
>（剧透: lighthouse 的报告显示8.5MB的 HTML 以200ms左右的时间优势胜出）
>
>【原内容】：Which has a better First Meaningful Paint time?
>- a raw 8.5MB HTML file with the full text of every single one of my 27,506 tweets
>- a client rendered React site with exactly one tweet on it
>
>(Spoiler: @____lighthouse reports 8.5MB of HTML wins by about 200ms)

花点时间想想这个问题，加载 **8.5兆** 的 HTML 竟然比用客户端 React 应用加载一条推文的速度更快。
## 最佳实践：包管理和模块打包
因为这些大的 JS 打包文件产生了性能问题，我们开始使用包管理器和模块打包工具，比如 Bower、Yard、Webpack、Parcel 和 Rollup。

它们处理了依赖管理，根据任何给定的页面上需要有什么来包含相应的 JS 文件（而不是在每个页面上加载所有的东西）。

新的包管理器还提供了一个叫做 "Tree Shaking" 的功能，在这个功能中，不使用和不需要包含的代码会被丢弃掉。

### 相依性地狱
这并不是一个糟糕的想法，但代价是高的配置和维护成本。

由于这些现代方法中的所有移动部件和深层的依赖关系，首次的配置和持续的维护可能是昂贵和耗时的。

让新的开发人员掌握使用你的代码库所需的一切，会成为一个复杂而微妙的平衡。有时，在新工作的第一周里，你都将与终端和配置调试打交道。

而且如果你的代码中某个地方的依赖关系过时了，你的整个构建就会崩溃掉。

### 复杂性的代价
那些本应帮助我们，节省我们时间的工具，最终往往因为一些微小的“小毛病”而让我们付出更多的时间。

Kyle Shevlin 在[推特](https://twitter.com/kyleshevlin/status/1095093908038549505)上说：

> 我今天的工作流程：15分钟写出能满足我需求的代码，2个小时的时间去试图安抚静态类型的“大神”。
>
> 【原内容】：My workflow today: 15 minutes of writing code that works and does what I want. 2 hours of trying to appease the static type gods.

下面是另一个来自才华横溢的 Brad Frost 的[例子](https://twitter.com/brad_frost/status/1097908466935652352)：

> 你会失望的意识到，你在现代 JS 框架中所苦苦挣扎的事情，在 jQuery 中可能只需要10分钟。
>
> 【原内容】：That depressing realization that the thing you’re slogging through in a modern JS framework could have taken you 10 minutes in jQuery.

Nicole Dominguez 分享了关于 Webpack 的类似[想法](https://twitter.com/sodevious/status/1126300690530357253)：

> 啊，Webpack，javascript，把一个1小时的项目配置变成8小时的事情。
>
> 【原内容】：ah webpack, javascript –– turning a 1 hour project setup into an 8 hour affair

JR Cook 痛定思痛[指出](https://twitter.com/Eldorian/status/1139555399160303616)：

> 去年我在团队中犯了一个错误，把我们正在开发的一个应用从仅仅使用 JQuery/Vanilla JS 推到了使用 Angular，因为我以为这会让我们的生活变得更简单。但事实并非如此，它只是增加了复杂性。
>
> 【原内容】：I made the mistake last year on my team to push one of the apps we were developing from just using JQuery/Vanilla JS to use Angular because I assumed it would make our lives easier. It did not, it just added complexity.

## 最佳实践：CSS-in-JS
CSS-in-JS 旨在解决在一个基于组件的设计系统中，CSS 对一个开发团队带来的一些挑战（这些开发团队有时对 JS 比 CSS 更熟悉）。

假设你有一个组件的样式是这样的：
```css
.callout {
    background-color: slategray;
    font-size: 2em;
};
```
```html
<div class="callout">
    👋 Hi there!
</div>
```
你的 UI 中的其他组件也有可能使用 `slategray` 的背景色或 `2em` 的字体大小。随着时间的推移，你的样式表中会有很多的冗余。

还有一种可能性是，你可能创建了一个带有一些样式的组件，后来被从项目中删除了，但却忘记了删除这些样式，这会导致项目变得更加的臃肿。

在 CSS-in-JS 中，你可以这样做：
```js
var callout = {
    backgroundColor: 'slategray',
    fontSize: '2em'
};
```
```html
<div class={callout}>
    👋 Hi there!
</div>
```
它看起来很相似，但使用 JS 约定而不是 CSS 约定。

这会渲染成类似下面的东西：
```html
<div class="jzrps nibae lupp mihax">
    👋 Hi there!
</div>
```
我们的想法是，每个属性都会被分解成自己的类并添加到元素中，这样你就可以减少冗余。

只有页面需要的才会被加载，这样你就可以避免“级联”将不需要的样式作用到其他元素上的问题。
### 更好的性能
前 Twitter 工程部的 Nicholas Gallagher 分享了 Twitter 从他们的传统代码库转向 CSS-in-JS 解决方案的这个[数据](https://twitter.com/necolas/status/1058949372837122048)：

> 对于每个样式主题，传统网站需要下载约630 KB CSS......PWA 会逐步生成约 30kb 的 CSS，并处理所有主题。
>
>【原内容】：Legacy site downloads ~630 KB CSS per theme and writing direction…
>PWA incrementally generates ~30 KB CSS that handles all themes and writing directions.

结果是令人印象深刻的，但是 630kb 大小的 CSS，对于 Twitter UI 的内容来说，感觉是不必要的。

正如我们稍后将看到的那样，他们的 PWA 所产生的代码实际上与其他一些纯粹基于 CSS 的技术是一样的，但是有大量额外的工具可供使用。

你最终得到的是人类完全无法读懂的类，这也使得调试UI问题变得更加困难。
### 技术门槛
这也有另一个后果：它将没有 JS 专业知识的人排除在外。

根据我的经验，拥有深厚专业的 CSS 知识的人并不总是对 JavaScript 有同样程度的适应性或熟练度（也不应该期望他们这样做）。

这些工具将那些有重要的、必不可少的贡献的人排除在外。

Alex Russell 是 Chrome 团队的一名开发人员。在他2018年的文章 [The Developer Bait & Switch](https://infrequently.org/2018/09/the-developer-experience-bait-and-switch/) 中，他谈到了人们围绕使用框架所做的稻草人争论：

> 以下是最近几次对话中的稻草人观点汇总：这些工具让我们的行动更快，因为我们可以更快地进行迭代，所以我们可以提供更好的体验。
>
>【原内容】：Here’s a straw-man composite from several recent conversations: These tools let us move faster. Because we can iterate faster we’re delivering better experiences.

对于一些开发者来说，这可能会改善他们的体验，特别是那些更愿意使用 JS 技术栈的人。

但是对于那些擅长 CSS 和语义化的 HTML，或者网络可访问性，或者用户交互模式的人来说，这可能会让他们以一种前所未有的方式被排除在开发流程之外。

### 技术门槛是有商业后果的
2018年，A11Y 顾问 Rian Rietveld 辞去了她作为 WordPress 可访问性团队负责人的职位，并在一篇详细的文章中记录了[原因](https://rianrietveld.com/2018/10/09/i-have-resigned-the-wordpress-accessibility-team/)：

> Gutenberg 是新的 WP 编辑器，它是建立在 React 之上的。
>
> 【原内容】：Gutenberg is the new WP editor, and it’s built on React.

正因为如此，也因为团队中没有人有 React 经验（他们也无法在 a11y 社区中找到志愿者），他们无法有效地自己进行改进。

这使得 Rian 和她的团队很难完成他们的工作任务。

2019年5月，a11y 对新的 Gutenberg 编辑器进行了详细的[审核](https://wpcampus.org/2019/05/gutenberg-audit-results/)，这是一份329页的报告，详细介绍了各种可访问性问题。光是执行摘要就有34页，它相当详细地记录了91个与可访问性相关的 bug。

如果 Rian 和她的团队没有因为技术上的选择而被排除在外的话，那么很多事情都是可以避免的。

这些工具可能会让你团队中的一些开发人员工作得更快，但我不相信它们会给你的用户带来更好的体验。

## 最佳实践：单页应用
我们还尝试过另一种方法来解决这个性能问题：单页应用。

在单页应用中，整个网站或应用存在于一个 HTML 文件中。JS 渲染内容，处理 URL 路由等。

只有部分内容会刷新，理论上来说，这比起要重新下载应用中某个特定页面所需的所有 JS 和 CSS 来说，性能更好。它还允许你创建花哨的页面过渡动画。

但这也打破了浏览器免费提供给你的一堆东西，你需要在 JS 中重新创建。

你需要：
- 拦截链接的点击事件
- 根据 URL 找出要显示的 HTML
- 更新地址栏中的 URL
- 处理前进/后退按钮的点击事件
- 更新网页标题
- 并将焦点转移到页面上

这些都是浏览器默认情况下可以开箱即用的东西，但现在感觉进入了一个恶性循环。

我们实际上是在用 JavaScript 去打破 Web 所提供的开箱即用的功能，以解决使用 JavaScript 所带来的性能问题，然后用更多的 JavaScript 去重新实现这些功能，并且所有的这些都是以性能的名义（但一开始，我们就使用 JavaScript 毁掉了它）。

就像我之前说的，这真的疯了!

### 脆弱性
我们对于 JavaScript 的过度依赖创造了一个非常脆弱的前端，最小的错误都可能会导致整个事情完全崩溃。

我们会遇到类似白屏死机的情况。

这是因为服务器发送的 HTML 仅仅由一个空的 `div` 组成，而“真正的内容”完全是用 JavaScript 渲染的，当这个文件由于某种原因失败时（或者只是还没有加载），你会什么都看不到。

是的，现在是2019年，JavaScript 是网络不可或缺的重要组成部分，大多数人不会禁用它。

但  CDN 会失败，在2019年7月，一个糟糕的部署使得 Cloudflare 服务宕机，财富1000强公司中有10%使用的是它的服务。没有 CDN，就没有 JS。

防火墙和广告拦截器对它们所能拦截的内容策略变得过于激进，我们发送的巨大的 JS 文件在网络不好时会直接超时。

Buzzfeed 的工程师 Ian Feather [分享](https://twitter.com/philhawksworth/status/990890920672456707)说，他们网站上约有1%的 JS 请求失败了，而他们每个月有1300万的请求数量。

人们在通勤时会使用移动设备浏览网络，但经过隧道时会失去连接。
### JavaScript 是技术栈中最脆弱的部分
很明显，没有人打算写出有 bug 的代码，但它时常会发生。而当它发生时，JavaScript 则是技术栈中最脆弱的部分。

如果你用大拇指在键盘上输入 `dvi` 而不是 `div`，浏览器会忽略你写的东西，把它当成 `div`，然后继续前进。

```html
<!-- Browsers render this... -->
<dvi></dvi>

<!-- Like this... -->
<div></div>
```

如果你打错了一个 CSS 属性，例如写的是 `bg-color` 而不是 `background-color`，浏览器就会忽略这个属性，然后继续前进。

```css
.hero {
    bg-color: #f7f7f7; /* The browser ignores this */
    width: 100%;
}
```
但假设你在 JS 中拼错了一个变量名，在下面的例子中把 `num` 写成了 `nmu`，JavaScript 文件会抛出一个错误，然后就停止了，整个项目会停止工作。

```js
var doubleIt = function (num) {
	// This will break all the things
    return nmu * 2;
};
```
HTML 和 CSS 会优雅地失败，JavaScript 则不会。
<div style="display:flex;justify-content:space-between">
<a href="./README.md">←上一章</a>
<a href="./how-did-we-get-here.md">下一章→</a>
</div>