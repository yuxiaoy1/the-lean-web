### 精益网络原则

在本章中
- 旧的并不意味着过时
- 原则1：拥抱平台
- 原则2：小型和模块化
- 原则3：网络是为所有人服务的
- 有公司真正使用精益网络原则吗？

让我们来看看一套新的最佳实践--精益网络原则，我们可以用它来构建一个更简单、更快速的网络。

**我想鼓励你，在某种程度上，成为一个开发者恐龙。**

作为一个行业，我们迷恋于闪亮的新事物。新技术、新工具、新趋势。这是让这个行业如此激动人心的原因，但也是造成你跟不上的感觉的原因。

#### 旧的并不意味着过时
旧的技术不会因为新的技术出来就失效。通常情况下，老办法比新办法更简单、更可靠。

例如，尽管汽车仍然存在，但自行车仍然是一个很好的交通选择。它们更容易维护。它们不需要汽油。它们更安静。你不需要那么多的训练就能使用自行车。他们可以去汽车不能去的地方。

我的山地车已经有20年历史了。每隔几年我就给轮胎充气，给链条上油，就这样。现在还能用。

**开发工具和技术也是如此。**

这并不意味着你永远不应该使用新的工具和方法。但我想鼓励你对你使用的工具和方法有更多的选择性，以及为什么要这样做。

对你和你的用户来说，实用性是否超过了使用它的成本？倚重于旧的和值得信赖的方法，但在对你和你的用户有利的时候，用新的工具和技术来增强它们。

#### 原则1：拥抱平台
与其使用依赖关系或库，不如使用本机的JavaScript方法和浏览器API，这些方法和API都是免费的。

过去很难的事情，比如在DOM中获取元素和操作类，现在真的很容易。

第一行是一些jQuery。第二行是等价的vanilla JS。

```js
// jQuery
$('#app').addClass('awesome');

// Vanilla JS
document.querySelector('#app').classList.add('awesome');
```
下面是我从React文档网站复制/粘贴的一些JSX代码......

```js
// JSX
var name = 'Josh Perez';
var element = <h1>Hello, {name}</h1>;

ReactDOM.render(
    element,
    document.getElementById('root')
);
```
......在vanilla JS中也是如此。
```js
// Vanilla JS
var name = 'Josh Perez';
var element = `<h1>Hello, ${name}</h1>`;

document.getElementById('root').innerHTML = element;
```
#### 使用HTML代替JavaScript
有时，该平台提供了HTML元素来处理一些原本需要JavaScript的东西，例如，详细信息和摘要元素创建了手风琴组件。

例如，`details`和`summary`元素可以创建手风琴组件1，它们可以被访问，可以被样式化，甚至可以发出一个事件，如果你想扩展它们，你可以用JavaScript挂入。

```html
<details>
	<summary>The toggle</summary>
	The content.
</details>
```
您可以使用一个简陋的`input`并将其与`datalist`元素关联起来，得到一个原生的HTML自动完成组件2。

```html
<label for="wizards">Who's the best wizard?</label>
<input type="text" id="wizards" name="wizards" list="wizards-list">
<datalist id="wizards-list">
	<option>Harry Potter</option>
	<option>Hermione</option>
	<option>Dumbledore</option>
	<option>Merlin</option>
	<option>Gandalf</option>
</datalist>
```
#### 使用CSS代替JavaScript
同样，CSS也可以处理很多你以前会使用JS的东西--尤其是动画。

丹尼尔-伊登（Daniel Eden）已经建立了一个完整的CSS动画库3，你可以从中偷学。

我最受欢迎的JS插件--也是我写过的最难的一个插件--是一个普通的JS平滑滚动动画插件4，它可以对页面上的锚链接进行动画滚动。

现在，一个单行的CSS属性也能做同样的事情：`scroll-behavior: smooth`。

```css
html {
	scroll-behavior: smooth;
}

@media screen and (prefers-reduced-motion: reduce) {
	html {
		scroll-behavior: auto;
	}
}
```
*(出于可访问性的原因，当`prefers-reduce-motion`被激活时，你应该禁用此行为。)*

#### 多页应用程序
路由是浏览器已经开箱即用的东西。单页JS应用破坏了这一点，所以我们添加了更多的JavaScript来把它放回去。

支持SPA的论点是，它们的性能更高，因为你避免了昂贵的页面重载。但还有另一种方法可以获得快速的页面加载并消除这种复杂性。

**你可以拥有一个多页面的应用，而不是一个单页面应用。**

每个页面都会渲染应用的不同部分，你可以让浏览器为你处理路由。如果你愿意，你仍然可以用JavaScript渲染内容。你只是在多个HTML文件中进行渲染，而不是一个文件。

这并不意味着你必须在服务器端渲染所有的标记或使用数据库驱动的网站。事实上，你最好不要这样做。

#### 静态HTML
当有人访问一个数据库驱动的网站的网页时，服务器会从数据库中提取内容，找出要使用的模板，然后将两者结合成一个HTML文件，再发送回来。

这需要时间，而且取决于你的服务器，可能需要很多时间。

而使用静态HTML文件，响应几乎是即时的。浏览器请求一个文件，然后就会得到一个文件。再加上首先发送较少的代码，你就可以得到近乎即时的页面加载。

我在我的学生使用的门户网站上使用这种方法6来访问他们的电子书和课程。

我用静态的HTML文件提供 "永远不会改变 "的标记（导航、标题等）。主体内容是针对用户和他们所购买的内容的。这些内容来自于一个API调用，并使用vanilla JS渲染。

尽管每个页面都是一个全新的HTML文档，但页面加载的感觉是即时的。

#### 静态网站生成器
如果你的网站或应用程序只是几个页面，手工创建静态HTML文件并不难。但是，如果您管理的是一个较大的项目，那么手工编写所有这些页面的代码将是不切实际的。

幸运的是，静态网站生成器为您提供了数据库驱动的网站的便利性，更简单的模板化，以及静态HTML文件的巨大性能优势。

像Hugo,7 Eleventy,8和Jekyll9这样的工具可以让你更容易地通过将模板和数据结合在一起创建许多HTML文件。

但它们是在用户访问你的网站之前提前完成的。

#### 那持久性数据呢？
围绕这个问题，我总是会遇到这样的问题。

> 那应该跨越多个视图的持久性数据呢？

我使用`sessionStorage`或`localStorage`来处理。
```js
var saveData = function (data) {
    sessionStorage.setItem(
        'myData',
        JSON.stringify(data)
    );
};

var getData = function () {
    var saved = sessionStorage.getItem('myData');
    if (saved) {
        return JSON.parse(saved);
    }
    return {};
};
```

#### 原则2：小型化和模块化
在我们这个行业里，伸手去拿多功能工具是很常见的做法。"看看所有的实用工具，都是内置的！" 功能是好的。

**当我们真正需要的是剪刀的时候，我们却去买多用途的工具刀。**

也许我们应该转而去接触那些只做好一件事的小而专注的工具（在这一点上，只有当单靠平台不行的时候）。

#### 框架替代方案
与其为了使用一两个函数（比如`_.groupBy()`）而包含27kb的lodash，你可以包含一个0.15kb（150字节）的辅助函数10来做同样的事情。

与其加载整个jQuery库(和jQuery UI)来添加一个toggle tabs插件，你可以加载一个建立在本地浏览器方法和API之上的插件11。

与其加载30kb以上的Angular，或React，或Vue，你可以使用hyperHTML，12在经过minifying和gzipping后，它只有不到一kb。或者Preact,13一个3kb的Reac替代品，拥有同样的现代API。

或者Reef,14，它只有2.5kb。或者Svelte15，或者Preact，16或者lit-html.17。

#### 面向对象的CSS
前面，我们谈到了CSS-in-JS。

它通过为你为特定组件设置的每一个单独的属性创建单独的类来帮助防止你的样式表中的冗余。

这是一个很好的方法，但你不需要一个JS工具来实现。

面向对象的CSS（或OOCSS）是由Nicole Sullivan18在几年前创建的。

如果你熟悉像Atomic CSS或实用性优先CSS这样的东西，它们是Nicole工作的衍生品（尽管她在这里很少得到她应得的荣誉）。   OOCSS对待CSS类就像FE开发人员的乐高玩具一样。

再看同一个组件，你不会直接对组件进行样式设计，而是为你要实现的目标创建实用类。这实际上和CSS-in-JS的例子是一样的结果，但是你可以读懂的类。

```css
.bg-gray {
    background-color: slategray;
};

.text-large {
    font-size: 2em;
}
```
```html
<div class="bg-gray text-large">
    👋 Hi there!
</div>
```

最初，这似乎会导致更多的CSS来完成同样的事情。对于任何一个组件来说，这可能是真的。

但是一旦你在一个项目中收集了一小批这样的CSS，你就可以开始在各个组件中混搭它们，以获得你想要的外观和感觉，而无需在你的代码库中添加更多的CSS。

这也有助于提高整个UI的一致性，减少每个组件在颜色、排版比例等方面的微小差异。

#### 原则3：网络是为每个人服务的
我主张只要能用，就用平台给你的东西。但这些原生功能并不是在每个设备或浏览器上都能使用。

过去，库和框架的一大优势是它们在不同的浏览器上实现了行为的标准化。

#### 使用Polyfills
你也可以使用polyfills来实现这一点：一些小的代码片段可以为不支持这些功能的浏览器添加支持。

下面是`Array.forEach()`方法的多边填充。

```js
if (!Array.prototype.forEach) {
  Array.prototype.forEach =
    function (callback, thisArg) {
      thisArg = thisArg || window;
        for (var i = 0; i < this.length; i++) {
            callback.call(thisArg, this[i], i, this);
        }
    };
}
```
它检查浏览器中是否已经存在该功能。如果不存在，它就会使用一些旧版浏览器支持的代码来复制本地功能。在这种情况下，它使用的是一个老式的for循环。

Polyfill.io,19来自《金融时报》团队，是一个免费服务，让使用polyfills变得超级简单。

```html
<script src="https://polyfill.io/v3/polyfill.min.js"></script>
```
在你的网站上包含它，就像你对待其他JS文件一样。

它会自动检测用户的浏览器，它只发送回用户需要的polyfills。在最新的Chrome上为0kb，在IE8上最小约为17kb，并进行了gzipped。

#### 分层构建
*渐进式增强*最近有点失宠了。

有这样一种思路，说人们不应该关闭浏览器中的JS，浏览器是免费的，所以他们可以随时升级到最新的浏览器。

但正如我们前面谈到的那样，大多数JS失败并不是因为有人故意禁用它，人们并不总是可以选择使用哪个浏览器。

渐进式增强，或分层构建，为你的网站或应用增加了Jeremy Keith所说的 "容错性 "20，并帮助确保尽可能多的人可以使用。

有一种观点认为，渐进式增强会增加很多工作，但不必如此。

例如，假设你想使用GitHub API和一些JavaScript在你的网站上显示你的仓库列表。与其使用一个空白的div，不如添加一个链接到你的GitHub账户。

```html
<div id="github">
	<a href="https://github.com/cferdinandi">View my projects on GitHub</a>
</div>
```
一旦加载了所需的JS，你就可以使用API来获取一个仓库列表，然后用GitHub的数据替换现有的链接。

如果该文件由于某种原因未能加载，人们仍然会得到一些可用的东西。

```html
<div id="github">
	<h3>My latest projects on GitHub</h3>
	<ul>
		<li><a href="https://github.com/cferdinandi/smooth-scroll">Smooth Scroll</a></li>
		<li><a href="https://github.com/cferdinandi/reef">Reef</a></li>
		<li><a href="https://github.com/cferdinandi/atomic">AtomicJS</a></li>
	</ul>
</div>
```

#### 渐进式的增强也适用于CSS
人们通常认为渐进式增强是JavaScript的事情，但它也适用于CSS。

CSS Grid让创建真正创新的布局变得更加容易，而这在过去是相当困难的。但是，它只适用于现代浏览器。IE和旧版本的Edge不支持它。

 但是......你可以把布局当做一种渐进式的增强。

支持它的浏览器将获得花哨的布局，而不支持它的浏览器将获得更简单的单列布局。

#### 包容性设计
在很多方面，网络是 "开箱即用 "的，而我们的选择却毁了它。

我们的设计和代码打破了使用键盘而不是鼠标导航的人在页面上移动的方式。我们的设计和代码打破了使用键盘而不是鼠标导航的人在页面上移动的方式，打破了屏幕阅读器消费内容的方式。

我们只关注最新的浏览器和更快的互联网连接，打破了低带宽国家和低收入家庭的大部分网络。

**网络是为每个人服务的，但我们经常建立网站和应用程序，好像它只为我们自己服务。**

这些东西可能很困难，我经常在无障碍性方面挣扎。一个令人惊奇的资源是A11Y项目21，它包含了大量的无障碍设计模式，还有一个你可以浏览的清单。

我也喜欢Dave Rupert的A11Y营养卡22。

它们包括键盘行为、焦点行为、以及各种JS驱动的组件所需的HTML元素和属性的快速概述。

#### 只要说 "不 "
构建包容性体验往往意味着对人们说 "不"，即使这样做很不舒服。

这意味着对你团队中的其他设计师或开发人员说 "不"，因为他们想以一种破坏一部分人的功能的方式来做事。这意味着对那些希望你做一些对用户不利的事情的客户说 "不"。

这意味着当你被告知要实现某个高管的宠物功能时，要对你的老板说 "不"。

很难说 "不"，但你是一个网络专家，你的职业义务就是大声说 "不"。

#### 有公司真正使用精益网络原则吗？#
每当我谈论这样的网站建设时，我通常会被问及是否有公司真的建立这样的网站和网络应用。

是的，他们有!

我在vanillaJSlist.com维护了一个以 "精益网络方式 "构建网站和应用的组织列表。23其中包括一些相当知名的公司。

在2018年底，GitHub从他们的应用程序中删除了jQuery.24，而不是用现代框架取代它，他们选择了本地浏览器方法和定制的Web组件。

我采访了参与该项目的开发人员之一Keith Cirkel。他告诉我，他们的内部座右铭之一是："像2005年一样建立网站"。

同样在2018年，Netflix将React从他们的默认前端页面加载中剥离出来（他们仍然使用React服务器端进行模板制作）25。

通过在客户端代码中使用vanilla JS，Netflix的Load Time和Time-to-Interactive降低了50%。

MeetSpace是一个视频消息应用。他们完全用vanilla JS编写了他们的应用，26因为他们希望体验尽可能的快。

创始人Nick Gauthier写道。

> 使用这种方法，我们能够创建一个令人难以置信的快速和轻量级的网络应用，而且随着时间的推移，维护工作也减少了。