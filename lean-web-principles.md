# 精益网络原则
章节目录：
- [旧的并不意味着过时](#旧的并不意味着过时)
- [原则1：拥抱平台](#原则1拥抱平台)
- [原则2：小型和模块化](#原则2小型和模块化)
- [原则3：网络是为所有人服务的](#原则3网络是为所有人服务的)
- [有公司真正使用精益网络原则吗？](#有公司真正使用精益网络原则吗？)

让我们来看看一套新的最佳实践：精益网络原则，我们可以用它来构建一个更简单、更快速的网络。

**我想鼓励你，在某种程度上，成为一个“开发者恐龙”。**

前端作为一个行业，我们会迷恋于闪亮的新事物。新技术、新工具、新趋势，这些都是让这个行业如此激动人心的原因，但也是造成你有种跟不上的感觉的原因。

## 旧的并不意味着过时
旧的技术不会因为新的技术出来就失效。通常情况下，老办法比新办法更简单、更可靠。

例如，尽管汽车仍然存在，但自行车仍然是一个很好的交通选择。它们更容易维护，并且不需要汽油，也更安静。你不需要那么多的训练就能使用自行车，它还可以去汽车不能去的地方。

我的山地车已经有20年历史了。每隔几年我就给轮胎充气，给链条上油，就这样。它现在还能用。

**开发工具和技术也是如此。**

这并不意味着你永远不应该使用新的工具和方法。但我想鼓励你对你使用的工具和方法有更多的选择性，以及为什么要这样做。

对你和你的用户来说，实用性是否超过了使用它的成本？倚重于旧的和值得信赖的方法，但在对你和你的用户有利的时候，用新的工具和技术来增强它们。

## 原则1：拥抱平台
与其使用依赖或库，不如使用本地的 JavaScript 方法和浏览器 API，这些方法和 API 随时都可以免费使用。

过去很难的事情，比如在DOM中获取元素和操作类，现在真的很容易。

第一行是 jQuery 实现，第二行是等价的 vanilla JS 实现。

```js
// jQuery
$('#app').addClass('awesome');

// Vanilla JS
document.querySelector('#app').classList.add('awesome');
```
下面是我从 React 文档网站复制的一些 JSX 代码：

```js
// JSX
var name = 'Josh Perez';
var element = <h1>Hello, {name}</h1>;

ReactDOM.render(
    element,
    document.getElementById('root')
);
```

使用 vanilla JS 则可以这样实现：

```js
// Vanilla JS
var name = 'Josh Perez';
var element = `<h1>Hello, ${name}</h1>`;

document.getElementById('root').innerHTML = element;
```
### 使用 HTML 代替 JavaScript
某些情况下，平台提供了 HTML 元素来处理一些原本需要使用 JavaScript 处理的东西。

例如，`details` 和 `summary` 元素可以创建[折叠面板组件](https://vanillajstoolkit.com/reference/javascript-free/accordions/)，它们可以被访问，可以被样式化，甚至如果你想扩展它们，你可以使用 JavaScript 挂载一些事件。

```html
<details>
	<summary>The toggle</summary>
	The content.
</details>
```

您可以使用一个简陋的 `input` 并将其与 `datalist` 元素关联起来，得到一个原生的 HTML [自动完成组件](https://vanillajstoolkit.com/reference/javascript-free/autocomplete/)：

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
### 使用 CSS 代替 JavaScript
同样，CSS 也可以处理很多你以前需要使用 JS 的东西，尤其是动画。

Daniel Eden 已经建立了一个完整的 [CSS 动画库](https://daneden.github.io/animate.css/)，你可以从中学习。

我最受欢迎的 JS 插件，也是我写过的最难的一个插件，是一个普通的 [JS 平滑滚动动画插件](https://github.com/cferdinandi/smooth-scroll)，在跳转到锚链接时可以进行平滑滚动。

现在，一个单行的 CSS 属性也能做同样的事情：[`scroll-behavior: smooth`](https://vanillajstoolkit.com/reference/javascript-free/smooth-scrolling/)。

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
*(出于可访问性的原因，当 `prefers-reduce-motion` 被激活时，你应该禁用此行为。)*

### 多页应用程序
路由是浏览器已有的开箱即用的东西，单页 JS 应用破坏了这一点，所以我们添加了更多的 JavaScript 来把它加回来。

支持 SPA 的论点是，它们的性能更高，因为你避免了昂贵的页面重载。但还有另一种方法可以获得快速的页面加载并消除这种复杂性。

**你可以拥有一个多页面的应用，而不是一个单页面应用。**

每个页面都会渲染应用的不同部分，你可以让浏览器为你处理路由。如果你愿意，你仍然可以用 JavaScript 渲染内容。你只是在多个 HTML 文件中进行渲染，而不是一个文件。

这并不意味着你必须在服务器端渲染所有的内容或使用数据库驱动开发网站。事实上，你最好不要这样做。
### 静态 HTML
当有人访问一个数据库驱动的网站的网页时，服务器会从数据库中提取内容，找出要使用的模板，然后将两者结合成一个 HTML 文件，再发送回来。

这需要时间，而且取决于你的服务器，所以可能会需要很多时间。

而使用静态 HTML 文件，响应几乎是即时的。浏览器请求一个文件，然后就会得到一个文件。再加上首次几乎不用发送代码，你可以得到近乎即时的页面加载。

我在我学生使用的[门户网站](https://courses.gomakethings.com)上使用这种方法来访问他们的电子书和课程。

我用静态的 HTML 文件提供“永远不会改变”的页面标记（导航、标题等），主体内容是针对用户和他们所购买的内容的。这些内容来自于一个 API 调用，并使用 vanilla JS 渲染。

尽管每个页面都是一个全新的 HTML 文档，但页面加载的感觉是即时的。
### 静态网站生成器
如果你的网站或应用程序只是几个页面，手工创建静态 HTML 文件并不难。但是，如果您管理的是一个较大的项目，那么手工编写所有这些页面的代码将是不切实际的。

幸运的是，静态网站生成器为您提供了数据库驱动的网站的便利性，更简单的模板化，以及静态 HTML 文件的巨大性能优势。

像 [Hugo](https://gohugo.io/)，[Eleventy](https://www.11ty.io/)，和 [Jekyll](https://jekyllrb.com/) 这样的工具可以让你更容易地将模板和数据结合在一起来创建许多的 HTML 文件。

但它们是需要在用户访问你的网站之前提前完成的。
### 那持久化数据呢？
我总是会遇到这样的问题：

那共享于多个视图间的持久化数据呢？

我使用 `sessionStorage` 或 `localStorage` 来处理：

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

## 原则2：小型化和模块化
在我们这个行业里，使用多功能的工具是很常见的做法。“看看所有的实用工具，都是内置的！”。

**当我们真正需要的是剪刀的时候，我们却去买多用途的工具刀。**

也许我们应该转而去接触那些只做好一件事的小而专注的工具（仅仅在只依靠平台的功能无法实现的时候）。

### 框架替代方案
与其仅仅为了使用一两个函数（比如 `_.groupBy()` ）而包含27kb的 lodash，你可以包含一个 0.15kb 的[辅助函数](https://vanillajstoolkit.com/helpers/groupby/)来做同样的事情。

与其加载整个 jQuery 库(和 jQuery UI )来添加一个 toggle tabs 插件，你可以加载一个建立在本地浏览器方法和 API 之上的[插件](https://github.com/cferdinandi/tabby)。

与其加载 30kb 以上的 Angular，或 React，或 Vue，你可以使用 [hyperHTML](https://viperhtml.js.org/hyperhtml/documentation/)，在经过 minifying 和 gzipping 后，它只有不到 1kb。或者 [Preact](https://preactjs.com/)，一个3kb的 React 替代品，拥有同样的现代 API。

或者 [Reef](https://github.com/cferdinandi/reef)，它只有2.5kb。或者 [Svelte](https://svelte.dev/)，或者 [lit-html](https://lit-html.polymer-project.org/)。

### 面向对象的CSS
前面，我们谈到了 CSS-in-JS。

它通过为特定组件设置的每一个单独的属性创建单独的类，来防止出现样式表的冗余。

这是一个很好的方法，但你不需要一个 JS 工具来实现。

[面向对象的 CSS](https://www.slideshare.net/stubbornella/object-oriented-css)（或 OOCSS ）是由 Nicole Sullivan 在几年前创建的。

如果你熟悉像 Atomic CSS 或实用性优先 CSS 这样的东西，它们是 Nicole 工作的衍生品（尽管她很少因此得到她应得的荣誉），OOCSS 对待 CSS 类就像前端开发人员的乐高玩具一样。

再看同一个组件，你不会直接对组件进行样式设计，而是为你要实现的目标创建实用类。这实际上和前面 CSS-in-JS 的例子是一样的结果，但是你可以读懂这些类。

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

最初，这似乎会导致需要编写更多的 CSS 来完成同样的事情。对于任何一个组件来说，这可能的确是这样。

但是一旦你在一个项目中收集了一小批这样的 CSS，你就可以开始在各个组件中混搭它们，以获得你想要的外观和感觉，而无需在你的代码库中添加更多的 CSS。

这也有助于提高整个 UI 的一致性，减少每个组件在颜色、排版比例等方面的微小差异。

## 原则3：网络是为每个人服务的
我主张只要可以用，就用平台提供的功能。但这些原生功能并不是在每个设备或浏览器上都能使用。

过去，库和框架的一大优势是它们在不同的浏览器上实现了行为的标准化。
### 使用 Polyfills
你也可以使用 polyfills 来实现这一点：一些小的代码片段可以为不支持这些功能的浏览器添加支持。

下面是 `Array.forEach()` 方法的 polyfill：

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
它检查浏览器中是否已经存在该功能，如果不存在，它就会使用一些旧版浏览器支持的代码来复制本地功能。在这种情况下，它使用的是一个老式的 for 循环。

[Polyfill.io](https://polyfill.io/)，来自 Financial Times 团队，它是一个免费服务，让使用 polyfills 变得超级简单。

```html
<script src="https://polyfill.io/v3/polyfill.min.js"></script>
```
在你的网站上引入它，就像引入其他 JS 文件一样。

它会自动检测用户的浏览器，并只返回用户需要的 polyfills。在最新的 Chrome 上为0kb，在 IE8 上最小约为 17kb，并进行了 gzip 压缩。

### 分层构建
“渐进式增强”最近有点失宠了。

有这样一种思路，说人们不应该禁用浏览器中的JS，浏览器是免费的，所以他们可以随时升级到最新版本。

但正如我们前面谈到的那样，大多数 JS 失败并不是因为有人故意禁用它，人们并不总是可以随意选择使用哪个浏览器。

渐进式增强，或分层构建，为你的网站或应用增加了 Jeremy Keith 所说的“[容错性](https://resilientwebdesign.com/)”，并确保尽可能多的人可以使用。

有一种观点认为，渐进式增强会增加很多工作，但未必如此。

例如，假设你想使用 GitHub API 和一些 JavaScript 在你的网站上显示你的仓库列表。与其使用一个空白的 div，不如添加一个链接到你的 GitHub 账户。

```html
<div id="github">
	<a href="https://github.com/cferdinandi">View my projects on GitHub</a>
</div>
```
一旦加载了所需的 JS，你就可以使用 API 来获取一个仓库列表，然后用 GitHub 的数据替换来现有的链接。

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
### 渐进式的增强也适用于 CSS
人们通常认为渐进式增强是 JavaScript 的事情，但它也适用于 CSS。

CSS Grid 让创建真正创新的布局变得更加容易，而这在过去是相当困难的。但是，它只适用于现代浏览器，IE 和旧版本的 Edge 并不支持它。

但是，你可以把布局当做一种渐进式的增强。

支持它的浏览器将获得花哨的布局，而不支持它的浏览器将获得更简单的单列布局。

### 包容性设计
在很多方面，网络是“开箱即用”的，而我们的选择却毁了它。

我们的设计和代码破坏了使用键盘而不是鼠标导航的人在页面上移动的方式，破坏了屏幕阅读器消费内容的方式。

我们只关注最新的浏览器和更快的互联网连接，却破坏了低带宽国家和低收入家庭的大部分网络体验。

**网络是为每个人服务的，但我们经常建立网站和应用程序，好像它只为我们自己服务。**

这些东西可能很困难，我经常在可访问性方面挣扎。一个令人惊奇的资源是 [A11Y 项目](https://a11yproject.com/)，它包含了大量的可访问性设计模式，还有一个你可以浏览的清单。

我也喜欢 Dave Rupert 的 [A11Y 营养卡](https://davatron5000.github.io/a11y-nutrition-cards/)。

它们包括键盘行为、焦点行为、以及各种 JS 驱动的组件所需的 HTML 元素和属性的快速概述。
### 只要说“不”
构建包容性体验往往意味着对人们说“不”，即使这样做很不舒服。

这意味着对你团队中的其他设计师或开发人员说“不”，因为他们想以一种破坏一部分人的功能的方式来做事。这意味着对那些希望你做一些对用户不利的事情的客户说“不”。

这意味着当你被告知要实现某个高管很感兴趣的功能时，要对你的老板说“不”。

很难说“不”，但你是一个专业的开发人员，你的职业义务就是大声说“不”。
## 有公司真正使用精益网络原则吗？#
每当我谈论这样的开发原则时，我通常会被问及是否有公司真的用这样的方式建立网站和网络应用。

有的!

我在 [vanillaJSlist.com](https://vanillajslist.com/) 维护了一个以“精益网络方法”构建网站和应用的组织列表，其中包括一些相当知名的公司。

在2018年底，GitHub 从他们的应用程序中[删除了 jQuery](https://githubengineering.com/removing-jquery-from-github-frontend/)，取而代之的不是现代框架，而是选择了本地浏览器方法和定制的Web组件。

我采访了参与该项目的开发人员之一 Keith Cirkel，他告诉我，他们的内部座右铭之一是：“像2005年一样建立网站”。

同样在2018年，Netflix 将 React 从他们的默认前端页面中[剥离出来](https://medium.com/dev-channel/a-netflix-web-performance-case-study-c0bcde26a9d9)（他们仍然使用 React 服务器端进行模板制作）。

通过在客户端代码中使用 vanilla JS，Netflix 的 Load Time 和 Time-to-Interactive 降低了50%。

MeetSpace 是一个视频消息应用。他们完全采用 vanilla JS 编写了他们的应用，因为他们希望[体验尽可能的快](https://www.smashingmagazine.com/2017/05/why-no-framework/)。

创始人 Nick Gauthier 写道：

> 使用这种方法，我们能够创建一个令人难以置信的快速和轻量级的网络应用，而且随着时间的推移，维护工作也减少了。
>
>【原内容】：Using this approach, we were able to create an incredibly fast and light web application that is also less work to maintain over time.

<div style="display:flex;justify-content:space-between">
<a href="./how-did-we-get-here.md">←上一章</a>
<a href="./what-now.md">下一章→</a>
</div>