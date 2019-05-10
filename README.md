# Puppeteer

<!-- [START badges] -->
[![Linux Build Status](https://img.shields.io/travis/com/GoogleChrome/puppeteer/master.svg)](https://travis-ci.com/GoogleChrome/puppeteer) [![Windows Build Status](https://img.shields.io/appveyor/ci/aslushnikov/puppeteer/master.svg?logo=appveyor)](https://ci.appveyor.com/project/aslushnikov/puppeteer/branch/master) [![Build Status](https://api.cirrus-ci.com/github/GoogleChrome/puppeteer.svg)](https://cirrus-ci.com/github/GoogleChrome/puppeteer) [![NPM puppeteer package](https://img.shields.io/npm/v/puppeteer.svg)](https://npmjs.org/package/puppeteer)
<!-- [END badges] -->

<img src="https://user-images.githubusercontent.com/10379601/29446482-04f7036a-841f-11e7-9872-91d1fc2ea683.png" height="200" align="right">

###### [API](https://github.com/GoogleChrome/puppeteer/blob/v1.15.0/docs/api.md) | [FAQ](#faq) | [Contributing](https://github.com/GoogleChrome/puppeteer/blob/master/CONTRIBUTING.md) | [Troubleshooting](https://github.com/GoogleChrome/puppeteer/blob/master/docs/troubleshooting.md)

> Puppeteer is a Node library which provides a high-level API to control Chrome or Chromium over the [DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/). Puppeteer runs [headless](https://developers.google.com/web/updates/2017/04/headless-chrome) by default, but can be configured to run full (non-headless) Chrome or Chromium.

<!-- [START usecases] -->
###### What can I do?

Most things that you can do manually in the browser can be done using Puppeteer! Here are a few examples to get you started:

* Generate screenshots and PDFs of pages.
* Crawl a SPA (Single-Page Application) and generate pre-rendered content (i.e. "SSR" (Server-Side Rendering)).
* Automate form submission, UI testing, keyboard input, etc.
* Create an up-to-date, automated testing environment. Run your tests directly in the latest version of Chrome using the latest JavaScript and browser features.
* Capture a [timeline trace](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference) of your site to help diagnose performance issues.
* Test Chrome Extensions.
<!-- [END usecases] -->

Give it a spin: https://try-puppeteer.appspot.com/

使用Javascript和HTML5分析和比较图像。更多信息＆Resemble.js演示。兼容Node.js> 8。

## Getting Started `快速开始`

### 一、 Installation `安装`

### 1.1 puppeteer

```bash
npm i puppeteer
# or "yarn add puppeteer"
```

安装这个版本的过程中，它下载了最新版本的Chromium（~170MB Mac，~282MB Linux，~280MB Win），可以保证与API一起使用。 要跳过下载，请查阅[Environment variables](https://github.com/GoogleChrome/puppeteer/blob/v1.15.0/docs/api.md#environment-variables).


### 1.2 puppeteer-core

从版本1.7.0开始，我们发布了 [`puppeteer-core`](https://www.npmjs.com/package/puppeteer-core)  一个Puppeteer版本，默认情况下不会下载Chromium。
这个版本默认情况下不会下载 Chromium， 它会忽略所有的 PUPPETEER_* 环境变量。

```bash
npm i puppeteer-core
# or "yarn add puppeteer-core"
```

puppeteer-core 旨在成为Puppeteer的轻量级版本，用于启动现有浏览器安装或连接到远程安装。确保您安装的puppeteer-core版本与您要连接的浏览器兼容。

请参阅  [puppeteer vs puppeteer-core](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#puppeteer-vs-puppeteer-core).

### 二、Usage  使用

注意：Puppeteer至少需要Node v6.4.0，但下面的示例使用async / await，它仅在Node v7.6.0或更高版本中受支持。

使用其他浏览器测试框架的人会熟悉Puppeteer。您创建一个Browser打开页面的实例，然后使用Puppeteer的 [ API](https://github.com/GoogleChrome/puppeteer/blob/v1.15.0/docs/api.md#). 对它们进行操作。

**示例** - navigating to https://example.com 并将截屏保存为example.png：

将文件另存为example.js

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');
  await page.screenshot({path: 'example.png'});

  await browser.close();
})();
```

在命令行上执行脚本

```bash
node example.js
```

Puppeteer将初始页面大小设置为800px x 600px，它定义了屏幕截图大小。页面大小可以自定义 [`Page.setViewport()`](https://github.com/GoogleChrome/puppeteer/blob/v1.15.0/docs/api.md#pagesetviewportviewport)

示例 - 创建PDF。

将文件另存为hn.js

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://news.ycombinator.com', {waitUntil: 'networkidle2'});
  await page.pdf({path: 'hn.pdf', format: 'A4'});

  await browser.close();
})();
```

在命令行上执行脚本

```bash
node hn.js
```

[`Page.pdf()`](https://github.com/GoogleChrome/puppeteer/blob/v1.15.0/docs/api.md#pagepdfoptions).有关创建pdf的更多信息，请参阅。

示例 - 在页面上下文中评估脚本

将文件另存为get-dimensions.js

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');

  // Get the "viewport" of the page, as reported by the page.
  const dimensions = await page.evaluate(() => {
    return {
      width: document.documentElement.clientWidth,
      height: document.documentElement.clientHeight,
      deviceScaleFactor: window.devicePixelRatio
    };
  });

  console.log('Dimensions:', dimensions);

  await browser.close();
})();
```

在命令行上执行脚本

```bash
node get-dimensions.js
```

节点get-dimensionss.js
请参阅 [`Page.evaluate()`](https://github.com/GoogleChrome/puppeteer/blob/v1.15.0/docs/api.md#pageevaluatepagefunction-args)有关详细信息，evaluate以及相关的方法，如evaluateOnNewDocument和exposeFunction。

<!-- [END getstarted] -->

<!-- [START runtimesettings] -->

## 三、 Default runtime settings运行时默认设置

**1. Uses Headless mode**

Puppeteer以  [headless mode](https://developers.google.com/web/updates/2017/04/headless-chrome) 推出Chromium 。要启动完整版Chromium，请在启动浏览器时  ['headless' option](https://github.com/GoogleChrome/puppeteer/blob/v1.15.0/docs/api.md#puppeteerlaunchoptions) 选项：

```js
const browser = await puppeteer.launch({headless: false}); // default is true
```

**2. 运行捆绑版的Chromium Runs a bundled version of Chromium** 

默认情况下，Puppeteer会下载并使用特定版本的Chromium，因此可以保证其API开箱即用。要将Puppeteer与不同版本的Chrome或Chromium一起使用，请在创建Browser实例时传入可执行文件的路径：

```js
const browser = await puppeteer.launch({executablePath: '/path/to/Chrome'});
```

See [`Puppeteer.launch()`](https://github.com/GoogleChrome/puppeteer/blob/v1.15.0/docs/api.md#puppeteerlaunchoptions) for more information.

有关[`this article`](https://www.howtogeek.com/202825/what%E2%80%99s-the-difference-between-chromium-and-chrome/) Chromium和Chrome之间差异的说明，请参阅。[`This article`](https://chromium.googlesource.com/chromium/src/+/master/docs/chromium_browser_vs_google_chrome.md)描述了Linux用户的一些差异。

**3. 创建新的用户配置文件

Puppeteer创建自己的Chromium用户配置文件，它可以在每次运行时清理。

<!-- [END runtimesettings] -->

## 四、资源Resources

- [API Documentation API文档](https://github.com/GoogleChrome/puppeteer/blob/v1.15.0/docs/api.md)
- [Examples 例子](https://github.com/GoogleChrome/puppeteer/tree/master/examples/)
- [Community list of Puppeteer resources Puppeteer资源的社区列表](https://github.com/transitive-bullshit/awesome-puppeteer)

<!-- [START debugging] -->

## 五、Debugging tips 调试技巧

1. 关闭 headless mode 模式 - 有时查看浏览器显示的内容很有用。使用headless: false以下方法启动完整版浏览器，而不是以 headless mode 模式启动 

        const browser = await puppeteer.launch({headless: false});

2. 减慢速度 - 该slowMo选项会使Puppeteer操作减慢指定的毫秒数。这是帮助了解正在发生的事情的另一种方式。

        const browser = await puppeteer.launch({
          headless: false,
          slowMo: 250 // slow down by 250ms
        });

3. 捕获控制台输出 - 您可以监听console事件。调试代码时，这也很方便page.evaluate()：

        page.on('console', msg => console.log('PAGE LOG:', msg.text()));

        await page.evaluate(() => console.log(`url is ${location.href}`));

4.在应用程序代码浏览器中使用调试器

  有两个执行上下文：运行测试代码的node.js和运行正在测试的应用程序代码的浏览器。这使您可以在应用程序代码浏览器中调试代码; 即内部代码evaluate()。

    - Use {devtools: true}启动Puppeteer时使用：

        `const browser = await puppeteer.launch({devtools: true});`

    - 更改默认测试超时：

        jest: `jest.setTimeout(100000);`

        jasmine: `jasmine.DEFAULT_TIMEOUT_INTERVAL = 100000;`

        mocha: `this.timeout(100000);` (don't forget to change test to use [function and not '=>'](https://stackoverflow.com/a/23492442))

    - 添加一个带有debuggerinside / add debugger到现有evaluate语句的evaluate语句：

      `await page.evaluate(() => {debugger;});`

      现在测试将在上面的评估语句中停止执行，并且chrome将在调试模式下停止。

5. 在node.js中使用调试器

    这将允许您调试测试代码。例如，您可以await page.click()在node.js脚本中单步执行，并在应用程序代码浏览器中看到单击。

    请注意，await page.click()由于此 [Chromium bug](https://bugs.chromium.org/p/chromium/issues/detail?id=833928).，
     您将无法在DevTools控制台中运行。因此，如果您想尝试一些东西，则必须将其添加到测试文件中。

- 添加debugger;到您的测试，例如：

      ```
      debugger;
      await page.click('a[target=_blank]');
      ```
    - Set `headless` to `false`
    - Run `node --inspect-brk`, eg `node --inspect-brk node_modules/.bin/jest tests`
    - In Chrome open `chrome://inspect/#devices` and click `inspect`
    - 在新打开的测试浏览器中，键入F8以恢复测试执行
    - 现在您debugger将被击中，您可以在测试浏览器中进行调试


6. 启用详细日志记录 - 将通过命名空间 [`debug`](https://github.com/visionmedia/debug)下的模块记录内部DevTools协议流量puppeteer。

        # Basic verbose logging
        env DEBUG="puppeteer:*" node script.js

        # Protocol traffic can be rather noisy. This example filters out all Network domain messages
        env DEBUG="puppeteer:*" env DEBUG_COLORS=true node script.js 2>&1 | grep -v '"Network'

7.使用 [ndb](https://github.com/GoogleChromeLabs/ndb) 轻松调试您的Puppeteer（节点）代码 Debug your Puppeteer (node) code easily, using 

  - npm install -g ndb（甚至更好，使用npx！） [npx](https://github.com/zkat/npx)!)

  - 添加debugger到您的Puppeteer（节点）代码

  - 在测试命令之前添加ndb（或npx ndb）。例如：

    ndb jest 或 ndb mocha（或npx ndb jest/ npx ndb mocha）

  - 像老板一样调试你的铬测试！


<!-- [END debugging] -->

## Contributing to Puppeteer 常问问题

Check out [contributing guide](https://github.com/GoogleChrome/puppeteer/blob/master/CONTRIBUTING.md) to get an overview of Puppeteer development.

<!-- [START faq] -->

# FAQ

#### Q: 谁维护Puppeteer？

Chrome DevTools团队负责维护这个仓库，但我们非常乐意为您提供项目帮助和专业知识！见 [Contributing](https://github.com/GoogleChrome/puppeteer/blob/master/CONTRIBUTING.md).

#### Q:Puppeteer 的目标和原则是什么？
 
该项目的目标是：

提供一个纤薄的规范库，突出了 [DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/). 的功能。
为类似的测试库提供参考实现。最终，这些其他框架可以采用Puppeteer作为其基础层。
增加无头/自动浏览器测试的采用。
帮助dogfood新的DevTools协议功能......并捕获错误！
详细了解自动浏览器测试的难点，并帮助填补这些空白。

我们采用Chromium原则来帮助我们推动产品决策： [Chromium principles](https://www.chromium.org/developers/core-principles)

速度：Puppeteer在自动化页面上的性能开销几乎为零。
安全性：Puppeteer针对Chromium进行关闭过程，使其可以安全地自动化潜在的恶意页面。
稳定性：Puppeteer不应该是片状的，不应该泄漏记忆。
简单性：Puppeteer提供易于使用，理解和调试的高级API。

#### Q: Puppeteer取代了Selenium / WebDriver吗？

不。这两个项目都有很多不同的原因：

Selenium / WebDriver专注于跨浏览器自动化; 它的价值主张是一个适用于所有主流浏览器的标准API。
Puppeteer专注于Chromium; 它的价值主张是更丰富的功能和更高的可靠性。

也就是说，您可以使用Puppeteer对Chromium运行测试，例如使用社区驱动的 [jest-puppeteer](https://github.com/smooth-code/jest-puppeteer).。虽然这可能不应该是您唯一的测试解决方案，但与WebDriver相比，它确实有一些优点：

Puppeteer需要零设置，并与最适合的Chromium版本捆绑在一起，使其非常容易入手 [very easy to start with](https://github.com/GoogleChrome/puppeteer/#getting-started).。在一天结束时，最好只进行一些仅使用铬的测试，而不是完全没有测试。
Puppeteer具有事件驱动的架构，可以消除很多潜在的瑕疵。在木偶剧剧本中不需要邪恶的“睡眠（1000）”调用。
Puppeteer默认运行无头，这使得它运行起来很快。Puppeteer v1.5.0还公开了浏览器上下文，从而可以有效地并行化测试执行。
Puppeteer在调试时会发光：将“无头”位翻转为false，添加“slowMo”，您将看到浏览器正在做什么。您甚至可以打开Chrome DevTools来检查测试环境。

#### Q: 为什么Puppeteer v.XXX不能与Chromium v​​.YYY合作？

我们认为Puppeteer是Chromium 不可分割的实体。每个版本的Puppeteer都捆绑了特定版本的Chromium - 这是唯一可以保证使用的版本。

这不是一个人为约束：关于Puppeteer的很多工作实际上都发生在Chromium存储库中。这是一个典型的故事：

报告了一个Puppeteer错误：https：//github.com/GoogleChrome/puppeteer/issues/2709
事实证明这是DevTools协议的一个问题，因此我们将其修复为Chromium：https：//chromium-review.googlesource.com/c/chromium/src/+/1102154
上游修复程序登陆后，我们将更新的Chromium更新为Puppeteer：https：//github.com/GoogleChrome/puppeteer/pull/2769
但是，通常需要将Puppeteer与官方谷歌Chrome而不是Chromium一起使用。为此，您应安装puppeteer-core与Chrome版本对应的版本。

例如，要使用chrome-71puppeteer -core驱动Chrome 71，请使用npm标记：

```bash
npm install puppeteer-core@chrome-71
```

#### Q: Puppeteer使用哪种Chromium版本？

Look for `chromium_revision` in [package.json](https://github.com/GoogleChrome/puppeteer/blob/master/package.json).

#### Q: 什么被认为是  “Navigation”

从Puppeteer的角度来看，“导航”是改变页面URL的任何东西。除了浏览器访问网络以从Web服务器获取新文档的常规导航  [anchor navigations](https://www.w3.org/TR/html5/single-page.html#scroll-to-fragid) and [History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) 之外，还包括锚定导航和历史API使用。

通过“导航”的定义，Puppeteer可以与单页应用程序无缝协作。

#### Q:  ““trusted" ”和“ "untrusted" ”输入事件之间有什么区别？

在浏览器中，输入事件可以分为两大类：可信对象和不可信对象。

可信事件：用户与页面交互生成的事件，例如使用鼠标或键盘。
不信任事件：通过网络的API，如生成的事件document.createEvent或element.click()方法。

网站可以区分这两组：

使用Event.isTrusted事件标志
嗅闻伴随事件。例如，每一个值得信赖的'click'事件  [`Event.isTrusted`](https://developer.mozilla.org/en-US/docs/Web/API/Event/isTrusted)  是由之前'mousedown'和'mouseup'事件。
出于自动化目的，生成可信事件非常重要。使用Puppeteer生成的所有输入事件都是受信任的，并且会触发相应的事件。如果由于某种原因，需要一个不受信任的事件，它总是可以跳到页面上下文page.evaluate并生成一个假事件：

```js
await page.evaluate(() => {
  document.querySelector('button[type=submit]').click();
});
```

#### Q: Puppeteer不支持哪些功能

在控制包含音频和视频的页面时，您可能会发现Puppeteer的行为不符合预期。（例如，视频播放/屏幕截图可能会失败。[video playback/screenshots is likely to fail](https://github.com/GoogleChrome/puppeteer/issues/291).)）有两个原因：

Puppeteer与Chromium捆绑在一起 - 而不是Chrome - 因此默认情况下，它继承了Chromium所有与媒体相关的限制。这意味着Puppeteer不支持AAC或H.264等许可格式。（但是，可以通过executablePath选项puppeteer.launch强制Puppeteer使用单独安装的Chrome版本而不是Chromium 。如果您需要支持这些媒体格式的官方Chrome版本，则应该只使用此配置。）
[Chromium's media-related limitations](https://www.chromium.org/audio-video).
[`executablePath` option to `puppeteer.launch`](https://github.com/GoogleChrome/puppeteer/blob/v1.15.0/docs/api.md#puppeteerlaunchoptions)
由于Puppeteer（在所有配置中）控制桌面版Chromium / Chrome，因此不支持仅由移动版Chrome支持的功能。这意味着Puppeteer 不支持HTTP直播流（HLS）。[does not support HTTP Live Streaming (HLS)](https://caniuse.com/#feat=http-live-streaming).

#### Q: 我在测试环境中安装/运行Puppeteer时遇到问题？

我们有各种操作系统的故障排除指南，列出了所需的依赖项。 [troubleshooting](https://github.com/GoogleChrome/puppeteer/blob/master/docs/troubleshooting.md) 

#### Q:我如何尝试/测试Puppeteer的预发布版本？

您可以查看此repo或从npm安装最新的预发布：

```bash
npm i --save puppeteer@next
```

请注意，预发布可能不稳定并且包含错​​误。

#### Q: 问：我还有更多问题！我在哪里问？

有很多方法可以获得Puppeteer的帮助：

错误追踪系统
堆栈溢出
松弛的渠道
- [bugtracker](https://github.com/GoogleChrome/puppeteer/issues)
- [stackoverflow](https://stackoverflow.com/questions/tagged/puppeteer)
- [slack channel](https://join.slack.com/t/puppeteer/shared_invite/enQtMzU4MjIyMDA5NTM4LTM1OTdkNDhlM2Y4ZGUzZDdjYjM5ZWZlZGFiZjc4MTkyYTVlYzIzYjU5NDIyNzgyMmFiNDFjN2UzNWU0N2ZhZDc)

在发布问题之前，请务必搜索这些频道。

<!-- [END faq] -->
