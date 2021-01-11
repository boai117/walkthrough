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

