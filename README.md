# Walkthrough

This is the walkthrough tutorial of SAPUI5 that in the [Demo Kit](http://veui5infra.dhcp.wdf.sap.corp:8080/sapui5-sdk-dist/#/topic/3da5f4be63264db99f2e5b04c5e853db).

Here it records every step of my learning.

## Note
1. `sap.ui.define([module1, module2], function (module1, module2) {});` 

这种加载 `module` 的方法用的是 `AMD (Asynchronous Module Definition)` 语法. 它异步加载所有列出的 `module` ，等到全部加载完成后立刻执行回调函数。

`sap.ui.define` 会定义一个 global namespace，这样整个 application 都可以引用，比如这里的 `App.controller.js` 返回的 controller。

`sap.ui.require` 也是异步加载某个依赖，但是不会定义一个 namespace。

2. `value="{/recipient/name}"` 叫做 `data binding`, 括号里面的 `/recipient/name` 定义了资源的路径.

3. `value="{/recipient/name}"` 是 `simple binding syntax`; `description="Hello {/recipient/name}"` 是 `complex binding syntax`. 为了支持后者，需要在 index.html 里显式设置 `data-sap-ui-compatVersion="edge"` or `data-sap-ui-bindingSyntax="complex"`.

4. `this.getView().setModel(oModel)` 这行代码是为了 view 里面能够使用 model。

5. `helloMsg=Hello {0}` 这是 `i18n` 里面的参数表示形式，多个参数以 `{0} {1} {2}` 的格式列出来。用到的地方以 `oBundle.getText("helloMsg", [para1, para2, para3])` 调用。

6. 运行 `SAPUI5` 应用时，它基于用户的浏览器语言设置加载相应的 `i18n_*.properties` 文件. 所有这些文件都是翻译同事从源文件 `i18n.properties` 翻译而来，包括英语。

7. `text="{i18n>showHelloButtonText}"` 语法从 `i18n` model 取出数据。

8. Components are independent and reusable parts used in SAPUI5 applications.

9. `Component` 里的 `init` 方法会在 `Component` 对象被创建的时候自动调用. 在 `Component` 里面定义了 root view，而不是之前那样在 `index.js` 里面创建 view。同时也把 `controller` 里面 `onInit` 方法做的事情移到这里做，设置 `model` 的时候不是 `this.getView().setModel(oModel);`, 而是 `this.setModel(oModel);`. 最后 `index.js` 里面创建 `ComponentContainer`.

10. `SAP Fiori launchpad` 作为 `application container` 创建 app 实例，不需要 local HTML 文件。里面所有的 app 都是以 `Component` 的形式实现的，然后以 `manifest.json` 文件作为配置文件，里面可以加载 `resource`，实例化 `model` 如 `i18n` 等。

11. `SAPUI5` 在 1.30 版本才开始支持在 `manifest.json` 文件中自动创建 `model` 实例。在那之前是手动在 `Component` 的 `init` 方法中实现的。

12. `"title": "{{appTitle}}"` 这是对资源文件的变量引用，不是数据绑定的语法。

13. `metadata : { manifest: "json" }` 这段 `Component` 的代码指定了实例化 `Component` 的时候自动加载和解析 `manifest.json` 文件。所以是加载顺序是先有 `Component.js` 再有 `manifest.json`。

14. `sap.m.App` 组件会在背后做以下事情：
    * 创建多个属性给 `index.html` 的 `header` 部分，用以在移动设备上显示得更好.
    * 提供功能以在不同的 `page` 之间做 `navigation`.

15. `Fiori launchpad` 已经有 `shell` 在 `Component UI` 周围, 不需要再添加 `Shell`.

16. `Fiori launchpad` 的 `SAPUI5` 版本是统一管理维护的，这种情况下，最好不要自己手动添加 `style.css`, 因为需要自己控制它在每个版本中都是ok的。

17. 不要在 `custom CSS` 中设定颜色，而是使用标准的 `theme-denpendent` 的 `class`.

18. `Fragment` 是轻量级 UI，可以被多个 `view` 重用，没有对应的 `controller`。其里面的事件处理函数可以放在所属 `view` 对应的 `controller`. `Dialog` 不属于任何 `view`，必须通过 `controller` 去实例化。

19. `Fragment` 本身不是 `control`，没有在 `DOM` 中存在，它只是个装 `controls` 的容器。

20. `Fragment.load` 创建 Fragment 实例，可以提供的参数是：
* `id`：所属 `view` 的 `id`
* `name`：`fragment` 的 `name`，就是其名字

21. `oView.addDependent(oDialog)` 把 `dialog` 放到了 `view` 的生命周期里。这样的好处是它随着 `view` 的销毁而销毁，不需要我们手动销毁这个对象。

22. 当 `Fragment` 里有事件处理函数的时候，比如里面有个 `button` 有 `press` 事件，需要把这个事件处理函数放到某个 `controller` 里面，可以在 `Fragment.load()` 的时候添加参数 `controller: this`。这样这个函数就可以放到这个对应的 `controller` 里了，然后通过 `this.xxx` 调用其他东西。

23. 在 `Component` 里面可以通过 `this.getRootControl()` 拿到 `root view`. `Controller` 里面可以通过 `this.getOwnerComponent()` 拿到对应的 `Component`.

24. `items="{invoice>/Invoices}"` 绝对路径，`title="{invoice>Quantity}"` 相对路径.

25. `Calculated Fields` 是一种允许绑定多个 `model` 中的不同属性到一个单一的 `control` 属性的绑定语法。

26. `Expression Binding` 表达式数据绑定方式，能够做简单的计算逻辑。