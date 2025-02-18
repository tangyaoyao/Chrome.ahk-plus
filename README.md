# Chrome.ahk-plus

Automate Google Chrome and MS Edge using native AutoHotkey.  
使用纯 AutoHotkey 操控 Chrome 和 Edge 。


## What has been enhanced compared to Chrome.ahk
* **Google Chrome** and **Microsoft Edge** are supported.
* Error reports directed to user code instead of library code.
* Added 30-seconds timeout for all funtions that could cause a dead loop.
* Simplified creation of ProfilePath.
* Fixed an issue that Chrome to report error due to slow opening.
* Fixed an issue that Chrome to report error due to shortcuts were not found.
* 基于 GeekDude 2023.03.21 Release 版修改，与 GeekDude 版相比有以下增强。
* 支持谷歌 Chrome 与微软 Edge 。
* 报错可直接定位到用户代码，而不是库代码。
* 为所有可能造成死循环的地方添加了默认30秒的超时参数。
* 简化了 Chrome 用户配置目录的创建。
* 修复了 Chrome 打开缓慢而报错的问题。
* 修复了找不到开始菜单中的 Chrome 快捷方式而报错的问题。


## How it works

Chrome offers a WebSocket based API they call the **Chrome DevTools Protocol**. This API is what allows web development tools to build integrations, and tools such as Selenium to perform their automation. The protocol's documentation describes a plethora of exciting endpoints accessible using this library, and can be found at the link below.

https://chromedevtools.github.io/devtools-protocol/


## Advantages

* **No external dependencies such as Selenium are required**
* Chrome can be automated even when running in headless mode
	* Launching in headless mode is not currently supported by this library
* Chrome consistently benchmarks better than Internet Explorer
* Chrome offers extensions which provide unique opportunities for interaction
	* Automate your Chromecast
	* Connect to remote servers with FoxyProxy and update web based configs
	* Manage your password vault with LastPass
* Many features are available that would be difficult to replicate in Internet Explorer
	* `Page.printToPDF`
	* `Page.captureScreenshot`
	* Geolocation spoofing


## Limitations

* Chrome **must** be started in debug mode
	* If chrome is already running out of debug mode, it must either be **closed and reopened** or **launched again under a new profile** that isn't already running
	* **You cannot attach to an existing non-debug session**
* Less flexible than Internet Explorer's COM interface
	* Cannot pass function references for callbacks


## Using this Library

To start using this library you need to create an instance of the class `Chrome`. `Chrome`'s constructor accepts four optional parameters:

1. **ProfilePath** - This is the path, relative to the working directory, that your Chrome user profile is located. If an empty folder is given, chrome will generate a new user profile in it. **When this parameter is omitted, Chrome will be launched under the default user profile.** However, if chrome is already running under that user profile out of debug mode, this will fail. Because of this, **it is recommended to always launch Chrome under an alternate user profile.**
2. **URLs** - The page or array of pages that chrome should initially be opened to. Pass an empty string to open Chrome's homepage. **When this parameter is omitted, Chrome will be opened to `about:blank`.**
3. **ChromePath** - The path to find the Chrome executable file. **When this parameter is omitted, Chrome will be launched from the path in its start menu entry.**
4. **DebugPort** - The network port to communicate with Chrome over. **When this parameter is omitted, port `9222` will be used** as specified in the Chrome DevTools Protocol documentation.

Once an instance of the class `Chrome` has been created, Google Chrome will be launched. To connect to the newly opened page call `PageInstance := ChromeInstance.GetPage()`. Afterward, use `PageInstance.Call()` to call protocol endpoints, and `PageInstance.Evaluate()` to execute JavaScript.

```AutoHotkey
#Include Chrome.ahk

; Create an instance of the Chrome class using
; the folder ChromeProfile to store the user profile
ChromeInst := new Chrome("ChromeProfile")

; Connect to the newly opened tab and navigate to another website
; Note: If your first action is to navigate away, it may be just as
; effective to provide the target URL when instantiating the Chrome class
PageInst := ChromeInst.GetPage()
PageInst.Call("Page.navigate", {"url": "https://autohotkey.com/"})
PageInst.WaitForLoad()

; Execute some JavaScript
PageInst.Evaluate("alert('Hello World!');")

; Close the browser (note: this closes *all* pages/tabs)
PageInst.Call("Browser.close")
PageInst.Disconnect()

ExitApp
return
```

**You can find more sample code showing how to use this library in the Examples folder.**
