# uiautomator2中文版

# uiautomator2


#前言
方便英文不好的朋友学习，上传此中文版文档，以下为原作者文档正文，QQ群等信息也未做更改如果有不准确的地方，欢迎各位大佬指正

[📖 查看英文版](README.md)

[![PyPI](https://img.shields.io/pypi/v/uiautomator2.svg)](https://pypi.python.org/pypi/uiautomator2)
![PyPI](https://img.shields.io/pypi/pyversions/uiautomator2.svg)
[![codecov](https://codecov.io/gh/openatx/uiautomator2/graph/badge.svg?token=d0ZLkqorBu)](https://codecov.io/gh/openatx/uiautomator2)


QQ交流群: **815453846**
Discord: <https://discord.gg/PbJhnZJKDd>

> 有段时间没有维护这个项目了（可能有两年了），但是最近工作需要又重新研究一下Android原生自动化，当然又调研了Appium，对比下来一看，发现uiautomator2这个项目的运行速度是真的好快，从检测元素到点击，都是毫秒级的，代码也比较好理解。真是没想到以前竟然写出了这么神奇的项目，这么好的项目怎么能让它落灰呢，得好好整一整，一些垃圾代码清理清理。所以项目版本从2.x.x升级到了3.x.x

还在用2.x.x版本的用户，可以先看一下[2to3](docs/2to3.md) 再决定是否要升级3.x.x （我个人还是非常建议升级的）

2到3毕竟是大版本升级，很多的函数删掉了。首先删掉的就是atx-agent，其次还有一堆atx-agent相关的函数。废弃的功能比如init.

各种依赖库的版本号

- [![PyPI](https://img.shields.io/pypi/v/uiautomator2.svg?label=uiautomator2)](https://pypi.python.org/pypi/uiautomator2)
- [![PyPI](https://img.shields.io/pypi/v/adbutils.svg?label=adbutils)](https://github.com/openatx/adbutils)
- [![GitHub tag (latest SemVer)](https://img.shields.io/github/tag/openatx/android-uiautomator-server.svg?label=android-uiautomator-server)](https://github.com/openatx/android-uiautomator-server)
- ~~[![GitHub tag (latest SemVer)](https://img.shields.io/github/tag/openatx/atx-agent.svg?label=atx-agent)](https://github.com/openatx/atx-agent)~~

[UiAutomator](https://developer.android.com/training/testing/ui-automator.html)是Google提供的用来做安卓自动化测试的一个Java库，基于Accessibility服务。功能很强，可以对第三方App进行测试，获取屏幕上任意一个APP的任意一个控件属性，并对其进行任意操作，但有两个缺点：1. 测试脚本只能使用Java语言 2. 测试脚本要打包成jar或者apk包上传到设备上才能运行。

我们希望测试逻辑能够用Python编写，能够在电脑上运行的时候就控制手机。这里要非常感谢 Xiaocong He ([@xiaocong][])，他将这个想法实现了出来（见[xiaocong/uiautomator](https://github.com/xiaocong/uiautomator)），原理是在手机上运行了一个http rpc服务，将uiautomator中的功能开放出来，然后再将这些http接口封装成Python库。
因为`xiaocong/uiautomator`这个库，已经很久不见更新。所以我们直接fork了一个版本，为了方便做区分我们就在后面加了个2 [openatx/uiautomator2](https://github.com/openatx/uiautomator2),对应的Android包源码我也fork了一份，[openatx/android-uiautomator-server](https://github.com/openatx/android-uiautomator-server)

除了对原有的库的bug进行了修复，还增加了很多新的Feature。主要有以下部分：

* ~~设备和开发机可以脱离数据线，通过WiFi互联（基于[atx-agent](https://github.com/openatx/atx-agent)~~
* ~~集成了[openstf/minicap](https://github.com/openstf/minicap)达到实时屏幕投频，以及实时截图~~
* ~~集成了[openstf/minitouch](https://github.com/openstf/minitouch)达到精确实时控制设备~~
* 修复了[xiaocong/uiautomator](https://github.com/xiaocong/uiautomator)经常性退出的问题
* 代码进行了重构和精简，方便维护
* 实现了一个设备管理平台(也支持iOS) [atxserver2](https://github.com/openatx/atxserver2) （注：目前不怎么维护了）
* 扩充了toast获取和展示的功能（需要手动开启ATX的悬浮窗权限） 貌似有bug用不了

>这里要先说明下，因为经常有很多人问 openatx/uiautomator2 并不支持iOS测试，需要iOS自动化测试，可以转到这个库 [openatx/facebook-wda](https://github.com/openatx/facebook-wda)。

> PS: 这个库 ~~<https://github.com/NeteaseGame/ATX>~~ 目前已经不维护了，请尽快更换。

这里有一份快速参考，适合已经入门的人 [QUICK REFERENCE GUIDE](QUICK_REFERENCE.md)，欢迎多提意见。

## 环境要求 (Requirements)
- Android版本 4.4+
- Python 3.8+

## 快速开始 (QUICK START)
先准备一台（不要两台）开启了`开发者选项`的安卓手机，连接上电脑，确保执行`adb devices`可以看到连接上的设备。

运行`pip3 install -U uiautomator2` 安装uiautomator2

命令行运行`python`打开python交互窗口。然后将下面的命令输入到窗口中。

```python
import uiautomator2 as u2

d = u2.connect() # 连接设备
print(d.info)
```

这时看到类似下面的输出，就可以正式开始用我们这个库了。因为这个库功能太多，后面还有很多的内容，需要慢慢去看 ....

```
{'currentPackageName': 'net.oneplus.launcher', 'displayHeight': 1920, 'displayRotation': 0, 'displaySizeDpX': 411, 'displaySizeDpY': 731, 'displayWidth': 1080, 'productName': 'OnePlus5', '
screenOn': True, 'sdkInt': 27, 'naturalOrientation': True}
```

另外为了保持稳定，还需要开启`小黄车`的悬浮窗权限。参考文章 [py-uiautomator2通过悬浮窗让服务长时间可用](https://zhuanlan.zhihu.com/p/688009468)

一般情况下都会成功，不过也可能会有意外。可以加QQ群反馈问题(群号在最上面），群里有很多大佬可以帮你解决问题。

## 赞助商 (Sponsors)
感谢所有赞助商! ✨🍰✨

### 金牌赞助商（Gold Sponsor）
Empty（暂无）

# 推荐文章 (Article Recommended)
优秀文章推荐 (欢迎QQ群里at我反馈）

- [termux里如何部署uiautomator2简介](https://www.cnblogs.com/ze-yan/p/12242383.html) by `成都-测试只会一点点`

## 相关项目 (Related projects)
- 基于adb协议与Android进行交互的库 [adbutils](https://github.com/openatx/adbutils)
- [uiauto.dev](https://uiauto.dev) 用于查看UI层级结构，类似于uiautomatorviewer(用于替代之前写的weditor），用于查看UI层级结构 
- 设备管理平台，设备多了就会用到 [atxserver2](https://github.com/openatx/atxserver2) （寻找项目维护人员）
- ~~[atx-agent](https://github.com/openatx/atx-agent) 运行在设备上的驻守程序，go开发，用于保活设备上相关的服务~~
- ~~[weditor](https://github.com/openatx/weditor) 类似于uiautomatorviewer，专门为本项目开发的辅助编辑器(这个暂不维护了~~

**[安装 (Installation)](#installation)**

**[连接设备 (Connect to a device)](#connect-to-a-device)**

**[命令行 (Command line)](#command-line)**

**[全局设置 (Global settings)](#global-settings)**
  - **[调试HTTP请求 (Debug HTTP requests)](#debug-http-requests)**
  - **[隐式等待 (Implicit wait)](#implicit-wait)**

**[应用管理 (App management)](#app-management)**
  - **[安装应用 (Install an app)](#install-an-app)**
  - **[启动应用 (Launch an app)](#launch-an-app)**
  - **[停止应用 (Stop an app)](#stop-an-app)**
  - **[停止所有运行的应用 (Stop all running apps)](#stop-all-running-apps)**
  - **[推送和拉取文件 (Push and pull files)](#push-and-pull-files)**
  - **[其他应用操作 (Other app operations)](#other-app-operations)**

**[UI自动化 (UI automation)](#basic-api-usages)**
  - **[Shell命令 (Shell commands)](#shell-commands)**
  - **[会话 (Session)](#session)**
  - **[获取设备信息 (Retrieve the device info)](#retrieve-the-device-info)**
  - **[按键事件 (Key Events)](#key-events)**
  - **[与设备的手势交互 (Gesture interaction with the device)](#gesture-interaction-with-the-device)**
  - **[屏幕相关 (Screen-related)](#screen-related)**
  - **[选择器 (Selector)](#selector)**
  - **[监视器 (Watcher)](#watcher)**
  - **[全局设置 (Global settings)](#global-settings)**
  - **[输入法 (Input method)](#input-method)**
  - **[Toast提示 (Toast)](#toast)**
  - **[XPath定位 (XPath)](#xpath)**
  - **[屏幕录制 (Screenrecord)](#screenrecord)**
  - **[图像匹配 (Image match) 已移除**


**[贡献者 (Contributors)](#contributors)**

**[许可证 (LICENSE)](#license)**


# 安装uiautomator2 (Installation)

1.安装uiautomator2

    ```bash
    pip install -U uiautomator2
    #-U参数表示升级到最新版本
    #该命令会从PyPI安装uiautomator2 Python包及其依赖项
    ```


测试安装是否成功：
```bash

    uiautomator2 --help
    #执行该命令可以查看uiautomator2的命令行帮助信息
    #如果显示帮助信息则说明安装成功
```    
2. UI检查器 (UI Inspector)

    ```bash
    #安装
    pip install uiautodev
    # 启动
    uiauto.dev
    ```

    浏览器打开 https://uiauto.dev 查看当前设备的界面结构。

    **uiauto.dev**

    [uiauto.dev](https://github.com/codeskyblue/uiauto.dev) 是一个独立与uiautomator2之外的一个项目，用于查看图层结构的。属于旧版项目[weditor的重构版本](https://github.com/openatx/weditor)，后续也许会收费（价格肯定物超所值），来支持当前这个项目继续维护下去。感兴趣的可以加群讨论(也包含提需求) QQ群 536481989 

# Connect to a device 连接指定设备
使用设备的序列号 (Serial) 连接设备，例如 `123456f`（可通过 `adb devices` 命令查看）

```python
import uiautomator2 as u2

d = u2.connect('123456f') # 等同于 u2.connect_usb('123456f')
print(d.info)
```

也可以通过环境变量 (env-var) `ANDROID_SERIAL` 传递序列号

```python
# export ANDROID_SERIAL=123456f
d = u2.connect()
```

# Command line 命令行
其中的`$device_ip`代表设备的IP地址

如需指定设备需要传入`--serial` 如 `python3 -m uiautomator2 --serial bff1234 <SubCommand>`, SubCommand为子命令（screenshot, current 等）

> 1.0.3 版本新增: `python3 -m uiautomator2` 等同于 `uiautomator2`

- screenshot: 截图

    ```bash
    $ uiautomator2 screenshot screenshot.jpg
    ```

- current: 获取当前包名和activity

    ```bash
    $ uiautomator2 current
    {
        "package": "com.android.browser",
        "activity": "com.uc.browser.InnerUCMobile",
        "pid": 28478
    }
    ```
    
- uninstall： 卸载应用 (Uninstall app)

    ```bash
    $ uiautomator2 uninstall <package-name> # 卸载一个包
    $ uiautomator2 uninstall <package-name-1> <package-name-2> # 卸载多个包
    $ uiautomator2 uninstall --all # 全部卸载
    ```

- stop: 停止应用 (Stop app)

    ```bash
    $ uiautomator2 stop com.example.app # 停止一个app
    $ uiautomator2 stop --all # 停止所有的app
    ```

- doctor: 诊断工具

    ```bash
    $ uiautomator2 doctor
    [I 2024-04-25 19:53:36,288 __main__:101 pid:15596] uiautomator2 is OK
    ```
    
# API Documents API文档

### New command timeout （已移除 Removed)
当Python退出时，UiAutomation服务也会退出。
<!-- How long (in seconds) will wait for a new command from the client before assuming the client quit and ending the uiautomator service （Default 3 minutes）

配置accessibility服务的最大空闲时间，超时将自动释放。默认3分钟。

```python
d.set_new_command_timeout(300) # change to 5 minutes, unit seconds
``` -->

### Debug HTTP requests 调试HTTP请求
打印出代码背后的HTTP请求信息

```python
>>> d.debug = True
>>> d.info
12:32:47.182 $ curl -X POST -d '{"jsonrpc": "2.0", "id": "b80d3a488580be1f3e9cb3e926175310", "method": "deviceInfo", "params": {}}' 'http://127.0.0.1:54179/jsonrpc/0'
12:32:47.225 Response >>>
{"jsonrpc":"2.0","id":"b80d3a488580be1f3e9cb3e926175310","result":{"currentPackageName":"com.android.mms","displayHeight":1920,"displayRotation":0,"displaySizeDpX":360,"displaySizeDpY":640,"displayWidth":1080,"productName"
:"odin","screenOn":true,"sdkInt":25,"naturalOrientation":true}}
<<< END
```

### Implicit wait 隐式等待
设置元素查找等待时间（默认20s）

```python
d.implicitly_wait(10.0) # 也可以通过d.settings['wait_timeout'] = 10.0 修改
d(text="Settings").click() # 如果10秒内没有找到Settings按钮，将抛出UiObjectNotFoundError异常

print("wait timeout", d.implicitly_wait()) # 获取默认的隐式等待时间
```

此函数会影响 `click`, `long_click`, `drag_to`, `get_text`, `set_text`, `clear_text` 等方法。

## App management 应用管理
这部分展示如何进行应用管理

### Install an app 安装应用
我们只支持从URL安装APK

```python
d.app_install('http://some-domain.com/some.apk')
```

### Launch an app 启动应用
```python
# 默认的这种方法是先通过atx-agent解析apk包的mainActivity，然后调用am start -n $package/$activity启动
d.app_start("com.example.hello_world")

# 使用 monkey -p com.example.hello_world -c android.intent.category.LAUNCHER 1 启动
# 这种方法有个副作用，它自动会将手机的旋转锁定给关掉
d.app_start("com.example.hello_world", use_monkey=True) # 通过包名启动

# 通过指定main activity的方式启动应用，等价于调用am start -n com.example.hello_world/.MainActivity
d.app_start("com.example.hello_world", ".MainActivity")
```

### Stop an app 停止应用
```python
# 等同于 `am force-stop`，可能会导致数据丢失
d.app_stop("com.example.hello_world") 
# 等同于 `pm clear`，清除应用数据
d.app_clear('com.example.hello_world')
```

### Stop all running apps 停止所有运行的应用
```python
# 停止所有应用
d.app_stop_all()
# 停止除com.examples.demo之外的所有应用
d.app_stop_all(excludes=['com.examples.demo'])
```

### Get app info 获取应用信息
```python
d.app_info("com.examples.demo")
# 预期输出
#{
#    "mainActivity": "com.github.uiautomator.MainActivity",
#    "label": "ATX",
#    "versionName": "1.1.7",
#    "versionCode": 1001007,
#    "size":1760809
#}

# 保存应用图标
img = d.app_icon("com.examples.demo")
img.save("icon.png")
```

### List all running apps 列出所有运行的应用
```python
d.app_list_running()
# 预期输出
# ["com.xxxx.xxxx", "com.github.uiautomator", "xxxx"]
```

### Wait until app running 等待应用运行
```python
pid = d.app_wait("com.example.android") # 等待应用运行, 返回进程ID(整数)
if not pid:
    print("com.example.android is not running")
else:
    print("com.example.android pid is %d" % pid)

d.app_wait("com.example.android", front=True) # 等待应用前台运行
d.app_wait("com.example.android", timeout=20.0) # 最长等待时间20s（默认）
```

> 在1.2.0版本中添加

### Push and pull files 推送和拉取文件
* 向设备推送文件 (push a file to the device)

    ```python
    # 推送到文件夹
    d.push("foo.txt", "/sdcard/")
    # 推送并重命名
    d.push("foo.txt", "/sdcard/bar.txt")
    # 推送文件对象
    with open("foo.txt", 'rb') as f:
        d.push(f, "/sdcard/")
    # 推送并修改文件访问权限
    d.push("foo.sh", "/data/local/tmp/", mode=0o755)
    ```

* 从设备拉取文件 (pull a file from the device)

    ```python
    d.pull("/sdcard/tmp.txt", "tmp.txt")

    # 如果设备上不存在该文件，将抛出FileNotFoundError异常
    d.pull("/sdcard/some-file-not-exists.txt", "tmp.txt")
    ```

### Other app operations 其他应用操作

```python
# 授予所有权限
d.app_auto_grant_permissions("io.appium.android.apis")

# 打开URL Scheme
d.open_url("appname://appnamehost")
# 等同于
# adb shell am start -a android.intent.action.VIEW -d "appname://appnamehost"
```

## Basic API Usages 基本API用法
这部分展示如何执行常见的设备操作：

### Shell commands Shell命令
* 运行一个短期Shell命令，带超时保护。（默认超时60秒）

    注意：超时支持需要 `atx-agent >=0.3.3`

    `adb_shell` 函数已废弃。请使用 `shell` 替代。

    简单用法：

    ```python
    output, exit_code = d.shell("pwd", timeout=60) # 超时60秒（默认）
    # output: "/\n", exit_code: 0
    # 类似于命令：adb shell pwd

    # 由于 `shell` 函数返回类型是 `namedtuple("ShellResponse", ("output", "exit_code"))`
    # 所以我们可以做一些技巧操作
    output = d.shell("pwd").output
    exit_code = d.shell("pwd").exit_code
    ```

    第一个参数可以是列表。例如：

    ```python
    output, exit_code = d.shell(["ls", "-l"])
    # output: "/....", exit_code: 0
    ```

   这将返回一个包含stdout和stderr合并内容的字符串。
   如果命令是阻塞命令，`shell` 也会阻塞直到命令完成或超时。在命令执行期间不会收到部分输出。此API不适合长时间运行的命令。给定的shell命令运行在类似于 `adb shell` 的环境中，具有 `adb` 或 `shell` 的Linux权限级别（高于应用权限）。

* 运行长时间Shell命令（已移除 Removed）
<!-- 
    添加 stream=True 将返回 `requests.models.Response` 对象。更多信息请参阅 [requests stream](http://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id5)

    ```python
    r = d.shell("logcat", stream=True)
    # r: requests.models.Response
    deadline = time.time() + 10 # 最多运行10秒
    try:
        for line in r.iter_lines(): # r.iter_lines(chunk_size=512, decode_unicode=None, delimiter=None)
            if time.time() > deadline:
                break
            print("Read:", line.decode('utf-8'))
    finally:
        r.close() # 必须调用此方法
    ```

    当调用 `r.close()` 时，命令将被终止。 -->

### Session 会话
会话代表一个应用的生命周期。可用于启动应用、检测应用崩溃。

* 启动和关闭应用 (Launch and close app)

    ```python
    sess = d.session("com.netease.cloudmusic") # 启动网易云音乐
    sess.close() # 停止网易云音乐
    sess.restart() # 冷启动网易云音乐
    ```

* 使用Python的`with`启动和关闭应用 (Use python `with` to launch and close app)

    ```python
    with d.session("com.netease.cloudmusic") as sess:
        sess(text="Play").click()
    ```

* 附加到正在运行的应用 (Attach to the running app)

    ```python
    # 如果应用未运行则启动，如果已运行则跳过启动
    sess = d.session("com.netease.cloudmusic", attach=True)
    ```

* 检测应用崩溃 (Detect app crash)

    ```python
    # 当应用仍在运行时
    sess(text="Music").click() # 操作正常进行

    # 如果应用崩溃或退出
    sess(text="Music").click() # 抛出SessionBrokenError异常
    # 会话下的其他函数调用也会抛出SessionBrokenError异常
    ```

    ```python
    # 检查会话是否正常
    # 警告：函数名可能在未来更改
    sess.running() # 返回True或False
    ```


### Retrieve the device info 获取设备信息

获取基本信息 (Get basic information)

```python
d.info
```

以下是可能的输出：

```
{'currentPackageName': 'com.android.systemui',
 'displayHeight': 1560,
 'displayRotation': 0,
 'displaySizeDpX': 360,
 'displaySizeDpY': 780,
 'displayWidth': 720,
 'naturalOrientation': True,
 'productName': 'ELE-AL00',
 'screenOn': True,
 'sdkInt': 29}
```

获取屏幕尺寸 (Get window size)

```python
print(d.window_size())
# 设备直立时的输出示例: (1080, 1920)
# 设备水平时的输出示例: (1920, 1080)
```

获取当前应用信息 (Get current app info)。对于某些安卓设备，输出可能为空（参见*输出示例3*）

```python
print(d.app_current())
# 输出示例1: {'activity': '.Client', 'package': 'com.netease.example', 'pid': 23710}
# 输出示例2: {'activity': '.Client', 'package': 'com.netease.example'}
# 输出示例3: {'activity': None, 'package': None}
```

等待特定Activity (Wait activity)

```python
d.wait_activity(".ApiDemos", timeout=10) # 默认超时10.0秒
# 输出: true或false
```

获取设备序列号 (Get device serial number)

```python
print(d.serial)
# 输出示例: 74aAEDR428Z9
```

获取WLAN IP地址 (Get WLAN ip)

```python
print(d.wlan_ip)
# 输出示例: 10.0.0.1或None
```


~~获取详细设备信息~~ `d.device_info`

设备信息 (device_info)

```python
print(d.device_info)
```

以下是可能的输出：

```
{'arch': 'arm64-v8a',
 'brand': 'google',
 'model': 'sdk_gphone64_arm64',
 'sdk': 34,
 'serial': 'EMULATOR34X1X19X0',
 'version': 14}
```

### Clipboard 剪贴板
获取或设置剪贴板内容 (Get of set clipboard content)

设置粘贴板内容或获取内容

* 剪贴板/设置剪贴板 (clipboard/set_clipboard)

    ```python
    d.clipboard = 'hello-world'
    # 或者
    d.set_clipboard('hello-world', 'label')

    ```

获取剪贴板内容 (Get clipboard content)

>  获取剪贴板内容需要IME(com.github.uiautomator/.AdbKeyboard)，使用前请调用 `d.set_input_ime()`

    ```python
    
    # 获取剪贴板内容
    print(d.clipboard)
    ```

### Key Events 按键事件

* 打开/关闭屏幕 (Turn on/off screen)

    ```python
    d.screen_on() # 打开屏幕
    d.screen_off() # 关闭屏幕
    ```

* 获取当前屏幕状态 (Get current screen status)

    ```python
    d.info.get('screenOn') # 需要Android >= 4.4
    ```

* 按硬键/软键 (Press hard/soft key)

    ```python
    d.press("home") # 按home键，使用键名称
    d.press("back") # 按back键，使用键名称
    d.press(0x07, 0x02) # 按键码0x07('0')，同时按下META ALT(0x02)
    ```

* 目前支持以下按键名称 (These key names are currently supported):

    - home（主页键）
    - back（返回键）
    - left（左键）
    - right（右键）
    - up（上键）
    - down（下键）
    - center（中心键）
    - menu（菜单键）
    - search（搜索键）
    - enter（回车键）
    - delete（或 del，删除键）
    - recent（最近任务键）
    - volume_up（音量增大键）
    - volume_down（音量减小键）
    - volume_mute（静音键）
    - camera（相机键）
    - power（电源键）

你可以在[Android KeyEvnet](https://developer.android.com/reference/android/view/KeyEvent.html)找到所有按键代码定义

* 解锁屏幕 (Unlock screen)

    ```python
    d.unlock()
    # 这相当于
    # 1. press("power")
    # 2. 从左下角滑动到右上角
    ```

### Gesture interaction with the device 与设备的手势交互
* 点击屏幕 (Click on the screen)

    ```python
    d.click(x, y)
    ```

* 双击 (Double click)

    ```python
    d.double_click(x, y)
    d.double_click(x, y, 0.1) # 两次点击之间的默认间隔是0.1秒
    ```

* 长按屏幕 (Long click on the screen)

    ```python
    d.long_click(x, y)
    d.long_click(x, y, 0.5) # 长按0.5秒（默认）
    ```

* 滑动 (Swipe)

    ```python
    d.swipe(sx, sy, ex, ey)
    d.swipe(sx, sy, ex, ey, 0.5) # 滑动0.5秒（默认）
    ```

* 滑动扩展功能 (SwipeExt)

    ```python
    d.swipe_ext("right") # 手指右滑，4选1 "left", "right", "up", "down"
    d.swipe_ext("right", scale=0.9) # 默认0.9, 滑动距离为屏幕宽度的90%
    d.swipe_ext("right", box=(0, 0, 100, 100)) # 在 (0,0) -> (100, 100) 这个区域做滑动

    # 实践发现上滑或下滑的时候，从中点开始滑动成功率会高一些
    d.swipe_ext("up", scale=0.8) # 代码会vkk

    # 还可以使用Direction作为参数
    from uiautomator2 import Direction
    
    d.swipe_ext(Direction.FORWARD) # 页面下翻, 等价于 d.swipe_ext("up"), 只是更好理解
    d.swipe_ext(Direction.BACKWARD) # 页面上翻
    d.swipe_ext(Direction.HORIZ_FORWARD) # 页面水平右翻
    d.swipe_ext(Direction.HORIZ_BACKWARD) # 页面水平左翻
    ```

* 拖动 (Drag)

    ```python
    d.drag(sx, sy, ex, ey)
    d.drag(sx, sy, ex, ey, 0.5) # 拖动0.5秒（默认）
    ```

* 滑动点序列 (Swipe points)

    ```python
    # 从点(x0, y0)滑动到点(x1, y1)再到点(x2, y2)
    # 每两点之间的滑动时间为0.2秒
    d.swipe_points([(x0, y0), (x1, y1), (x2, y2)], 0.2))
    ```

    多用于九宫格解锁，提前获取到每个点的相对坐标（这里支持百分比），
    更详细的使用参考这个帖子 [使用u2实现九宫图案解锁](https://testerhome.com/topics/11034)

* 触摸和拖动（测试版） (Touch and drap (Beta))

    这个接口属于比较底层的原始接口，感觉并不完善，不过凑合能用。注：这个地方并不支持百分比

    ```python
    d.touch.down(10, 10) # 模拟按下
    time.sleep(.01) # down 和 move 之间的延迟，自己控制
    d.touch.move(15, 15) # 模拟移动
    d.touch.up(10, 10) # 模拟抬起
    ```

注意：click, swipe, drag操作支持百分比位置值。例如：

`d.long_click(0.5, 0.5)` 表示在屏幕中心长按

### Screen-related 屏幕相关
* 获取/设置设备方向 (Retrieve/Set device orientation)

    可能的方向：

    -   `natural` 或 `n`（自然方向）
    -   `left` 或 `l`（左侧）
    -   `right` 或 `r`（右侧）
    -   `upsidedown` 或 `u`（上下颠倒，不能设置）

    ```python
    # 获取方向。输出可能是 "natural" 或 "left" 或 "right" 或 "upsidedown"
    orientation = d.orientation

    # 警告：在我的TT-M1上测试未通过
    # 设置方向并冻结旋转
    # 注意：设置"upsidedown"需要Android>=4.3
    d.set_orientation('l') # 或 "left"
    d.set_orientation("l") # 或 "left"
    d.set_orientation("r") # 或 "right"
    d.set_orientation("n") # 或 "natural"
    ```

* 冻结/解冻旋转 (Freeze/Un-freeze rotation)

    ```python
    # 冻结旋转
    d.freeze_rotation()
    # 解冻旋转
    d.freeze_rotation(False)
    ```

* 截图 (Take screenshot)

    ```python
    # 截图并保存到电脑上的文件，需要Android>=4.2
    d.screenshot("home.jpg")
    
    # 获取PIL.Image格式的图像。当然，你需要先安装pillow
    image = d.screenshot() # 默认format="pillow"
    image.save("home.jpg") # 或home.png。目前只支持png和jpg格式

    # 获取opencv格式的图像。当然，你需要先安装numpy和cv2
    import cv2
    image = d.screenshot(format='opencv')
    cv2.imwrite('home.jpg', image)

    # 获取原始jpeg数据
    imagebin = d.screenshot(format='raw')
    open("some.jpg", "wb").write(imagebin)
    ```

* 导出UI层次结构 (Dump UI hierarchy)

    ```python
    # 获取UI层次结构的dump内容
    xml = d.dump_hierarchy()

    # compressed=True: 包含不重要的节点
    # pretty: 格式化xml
    # max_depth: 限制xml深度，默认50
    xml = d.dump_hierarchy(compressed=False, pretty=False, max_depth=50)
    ```

* 打开通知栏或快速设置 (Open notification or quick settings)

    ```python
    d.open_notification()
    d.open_quick_settings()
    ```

### Selector 选择器

选择器是一种便捷的机制，用于在当前窗口中识别特定的UI对象。

```python
# 选择文本为'Clock'且className为'android.widget.TextView'的对象
d(text='Clock', className='android.widget.TextView')
```

选择器支持以下参数。有关详细信息，请参阅[UiSelector Java文档](http://developer.android.com/tools/help/uiautomator/UiSelector.html)。

*  `text`, `textContains`, `textMatches`, `textStartsWith`（文本相关）
*  `className`, `classNameMatches`（类名相关）
*  `description`, `descriptionContains`, `descriptionMatches`, `descriptionStartsWith`（描述相关）
*  `checkable`, `checked`, `clickable`, `longClickable`（可操作性相关）
*  `scrollable`, `enabled`,`focusable`, `focused`, `selected`（状态相关）
*  `packageName`, `packageNameMatches`（包名相关）
*  `resourceId`, `resourceIdMatches`（资源ID相关）
*  `index`, `instance`（索引和实例相关）

#### Children and siblings 子元素和兄弟元素

* 子元素 (children)

  ```python
  # 获取子元素或孙元素
  d(className="android.widget.ListView").child(text="Bluetooth")
  ```

* 兄弟元素 (siblings)

  ```python
  # 获取兄弟元素
  d(text="Google").sibling(className="android.widget.ImageView")
  ```

* 通过文本、描述或实例获取子元素 (children by text or description or instance)

  ```python
  # 获取符合条件className="android.widget.LinearLayout"的子元素
  # 以及其子元素或孙元素中文本为"Bluetooth"的元素
  d(className="android.widget.ListView", resourceId="android:id/list") \
   .child_by_text("Bluetooth", className="android.widget.LinearLayout")

  # 通过允许滚动搜索来获取子元素
  d(className="android.widget.ListView", resourceId="android:id/list") \
   .child_by_text(
      "Bluetooth",
      allow_scroll_search=True,
      className="android.widget.LinearLayout"
    )
  ```

  - `child_by_description` 是查找其孙元素具有指定描述的子元素，其他参数与 `child_by_text` 类似。

  - `child_by_instance` 是查找在其子层次结构中任何位置具有指定实例的子UI元素。它在可见视图上执行**不滚动**。

  有关详细信息，请参阅以下链接：

  -   [UiScrollable](http://developer.android.com/tools/help/uiautomator/UiScrollable.html), `getChildByDescription`, `getChildByText`, `getChildByInstance`
  -   [UiCollection](http://developer.android.com/tools/help/uiautomator/UiCollection.html), `getChildByDescription`, `getChildByText`, `getChildByInstance`

  上述方法支持链式调用，例如，对于以下层次结构

  ```xml
  <node index="0" text="" resource-id="android:id/list" class="android.widget.ListView" ...>
    <node index="0" text="WIRELESS & NETWORKS" resource-id="" class="android.widget.TextView" .../>
    <node index="1" text="" resource-id="" class="android.widget.LinearLayout" ...>
      <node index="1" text="" resource-id="" class="android.widget.RelativeLayout" ...>
        <node index="0" text="Wi‑Fi" resource-id="android:id/title" class="android.widget.TextView" .../>
      </node>
      <node index="2" text="ON" resource-id="com.android.settings:id/switchWidget" class="android.widget.Switch" .../>
    </node>
    ...
  </node>
  ```
  ![settings](https://raw.github.com/xiaocong/uiautomator/master/docs/img/settings.png)

  要点击位于文本'Wi‑Fi'右侧的开关部件，我们需要先选择开关部件。然而，根据UI层次结构，存在多个几乎具有相同属性的开关部件。通过className选择不会生效。或者，以下选择策略将有所帮助：

  ```python
  d(className="android.widget.ListView", resourceId="android:id/list") \
    .child_by_text("Wi‑Fi", className="android.widget.LinearLayout") \
    .child(className="android.widget.Switch") \
    .click()
  ```

* 相对位置 (relative positioning)

  我们还可以使用相对定位方法来获取视图：`left`, `right`, `top`, `bottom`。

  -   `d(A).left(B)`，选择A左侧的B。
  -   `d(A).right(B)`，选择A右侧的B。
  -   `d(A).up(B)`，选择A上方的B。
  -   `d(A).down(B)`，选择A下方的B。

  所以对于上述情况，我们可以替代性地使用：

  ```python
  ## 选择"Wi‑Fi"右侧的"switch"
  d(text="Wi‑Fi").right(className="android.widget.Switch").click()
  ```

* 多个实例 (Multiple instances)

  有时屏幕可能包含具有相同属性的多个视图，例如文本，这时你需要使用选择器中的"instance"属性来选择符合条件的实例之一，如下所示：

  ```python
  d(text="Add new", instance=0)  # 表示文本为"Add new"的第一个实例
  ```

  此外，uiautomator2提供了类似列表的API（类似于jQuery）：

  ```python
  # 获取当前屏幕上文本为"Add new"的视图数量
  d(text="Add new").count

  # 与count属性相同
  len(d(text="Add new"))

  # 通过索引获取实例
  d(text="Add new")[0]
  d(text="Add new")[1]
  ...

  # 迭代器
  for view in d(text="Add new"):
      view.info  # ...
  ```

  **注意**：在使用在结果列表中遍历的代码块中使用选择器时，必须确保屏幕上的UI元素保持不变。否则，在迭代列表时可能会发生Element-Not-Found错误。

#### 获取所选UI对象的状态及其信息 (Get the selected ui object status and its information)
* 检查特定UI对象是否存在 (Check if the specific UI object exists)

    ```python
    d(text="Settings").exists # 如果存在则为True，否则为False
    d.exists(text="Settings") # 上述属性的别名

    # 高级用法
    d(text="Settings").exists(timeout=3) # 等待Settings在3秒内出现，相当于.wait(3)
    ```

* 获取特定UI对象的信息 (Retrieve the info of the specific UI object)

    ```python
    d(text="Settings").info
    ```

    以下是可能的输出：

    ```
    { u'contentDescription': u'',
    u'checked': False,
    u'scrollable': False,
    u'text': u'Settings',
    u'packageName': u'com.android.launcher',
    u'selected': False,
    u'enabled': True,
    u'bounds': {u'top': 385,
                u'right': 360,
                u'bottom': 585,
                u'left': 200},
    u'className': u'android.widget.TextView',
    u'focused': False,
    u'focusable': True,
    u'clickable': True,
    u'chileCount': 0,
    u'longClickable': True,
    u'visibleBounds': {u'top': 385,
                        u'right': 360,
                        u'bottom': 585,
                        u'left': 200},
    u'checkable': False
    }
    ```

* 获取/设置/清除可编辑字段的文本（例如EditText小部件） (Get/Set/Clear text of an editable field (e.g., EditText widgets))

    ```python
    d(text="Settings").get_text()  # 获取小部件文本
    d(text="Settings").set_text("我的文本...")  # 设置文本
    d(text="Settings").clear_text()  # 清除文本
    ```

* 获取小部件中心点 (Get Widget center point)

    ```python
    x, y = d(text="Settings").center()
    # x, y = d(text="Settings").center(offset=(0, 0)) # 左上角的x, y
    ```
    
* 对小部件进行截图 (Take screenshot of widget)

    ```python
    im = d(text="Settings").screenshot()
    im.save("settings.jpg")
    ```

#### 对所选UI对象执行点击操作 (Perform the click action on the selected UI object)
* 在特定对象上执行点击 (Perform click on the specific object)

    ```python
    # 在指定UI对象的中心点击
    d(text="Settings").click()
    
    # 最多等待10秒直到元素出现，然后点击
    d(text="Settings").click(timeout=10)
    
    # 使用偏移量点击(x_offset, y_offset)
    # click_x = x_offset * width + x_left_top
    # click_y = y_offset * height + y_left_top
    d(text="Settings").click(offset=(0.5, 0.5)) # 默认中心点
    d(text="Settings").click(offset=(0, 0)) # 点击左上角
    d(text="Settings").click(offset=(1, 1)) # 点击右下角

    # 当元素在10秒内存在时点击，默认超时0秒
    clicked = d(text='Skip').click_exists(timeout=10.0)
    
    # 点击直到元素消失，返回布尔值
    is_gone = d(text="Skip").click_gone(maxretry=10, interval=1.0) # maxretry默认10，interval默认1.0
    ```

* 在特定UI对象上执行长按 (Perform long click on the specific UI object)

    ```python
    # 在指定UI对象的中心长按
    d(text="Settings").long_click()
    ```

#### 针对特定UI对象的手势操作 (Gesture actions for the specific UI object)
* 将UI对象拖动到另一个点或另一个UI对象 (Drag the UI object towards another point or another UI object)

    ```python
    # 注意：拖动不能用于Android<4.3
    # 在0.5秒内将UI对象拖动到屏幕点(x, y)
    d(text="Settings").drag_to(x, y, duration=0.5)
    # 在0.25秒内将UI对象拖动到另一个UI对象（的中心位置）
    d(text="Settings").drag_to(text="Clock", duration=0.25)
    ```

* 从UI对象的中心向其边缘滑动 (Swipe from the center of the UI object to its edge)

    滑动支持4个方向：

    - left（左）
    - right（右）
    - top（上）
    - bottom（下）

    ```python
    d(text="Settings").swipe("right")
    d(text="Settings").swipe("left", steps=10)
    d(text="Settings").swipe("up", steps=20) # 1步约5毫秒，所以20步约0.1秒
    d(text="Settings").swipe("down", steps=20)
    ```

* 从一点到另一点的双指手势 (Two-point gesture from one point to another)

  ```python
  d(text="Settings").gesture((sx1, sy1), (sx2, sy2), (ex1, ey1), (ex2, ey2))
  ```

* 对特定UI对象执行双指手势 (Two-point gesture on the specific UI object)

  支持两种手势：
  - `In`，从边缘到中心
  - `Out`，从中心到边缘

  ```python
  # 注意：捏合直到Android 4.3才能设置
  # 从边缘到中心。这里是"In"而不是"in"
  d(text="Settings").pinch_in(percent=100, steps=10)
  # 从中心到边缘
  d(text="Settings").pinch_out()
  ```

* 等待特定UI出现或消失 (Wait until the specific UI appears or disappears)
    
    ```python
    # 等待UI对象出现
    d(text="Settings").wait(timeout=3.0) # 返回布尔值
    # 等待UI对象消失
    d(text="Settings").wait_gone(timeout=1.0)
    ```

    默认超时为20秒。有关更多详细信息，请参阅**全局设置**

* 在特定UI对象上执行快速滑动（可滚动） (Perform fling on the specific ui object(scrollable))

  可能的属性：
  - `horiz`（水平）或 `vert`（垂直）
  - `forward`（向前）或 `backward`（向后）或 `toBeginning`（到开始）或 `toEnd`（到结束）

  ```python
  # 默认垂直向前快速滑动
  d(scrollable=True).fling()
  # 水平向前快速滑动
  d(scrollable=True).fling.horiz.forward()
  # 垂直向后快速滑动
  d(scrollable=True).fling.vert.backward()
  # 水平快速滑动到开始
  d(scrollable=True).fling.horiz.toBeginning(max_swipes=1000)
  # 垂直快速滑动到结束
  d(scrollable=True).fling.toEnd()
  ```

* 在特定UI对象上执行滚动（可滚动） (Perform scroll on the specific ui object(scrollable))

  可能的属性：
  - `horiz`（水平）或 `vert`（垂直）
  - `forward`（向前）或 `backward`（向后）或 `toBeginning`（到开始）或 `toEnd`（到结束），或 `to`（到某处）

  ```python
  # 默认垂直向前滚动
  d(scrollable=True).scroll(steps=10)
  # 水平向前滚动
  d(scrollable=True).scroll.horiz.forward(steps=100)
  # 垂直向后滚动
  d(scrollable=True).scroll.vert.backward()
  # 水平滚动到开始
  d(scrollable=True).scroll.horiz.toBeginning(steps=100, max_swipes=1000)
  # 垂直滚动到结束
  d(scrollable=True).scroll.toEnd()
  # 垂直向前滚动直到特定UI对象出现
  d(scrollable=True).scroll.to(text="Security")
  ```

### WatchContext 监视上下文
目前的这个watch_context是用threading启动的，每2s检查一次
目前还只有click这一种触发操作

```python
with d.watch_context() as ctx:
    # 当同时出现 （立即下载 或 立即更新）和 取消 按钮的时候，点击取消
    ctx.when("^立即(下载|更新)").when("取消").click() 
    ctx.when("同意").click()
    ctx.when("确定").click()
    # 上面三行代码是立即执行完的，不会有什么等待
    
    ctx.wait_stable() # 开启弹窗监控，并等待界面稳定（两个弹窗检查周期内没有弹窗代表稳定）

    # 使用call函数来触发函数回调
    # call 支持两个参数，d和el，不区分参数位置，可以不传参，如果传参变量名不能写错
    # eg: 当有元素匹配仲夏之夜，点击返回按钮
    ctx.when("仲夏之夜").call(lambda d: d.press("back"))
    ctx.when("确定").call(lambda el: el.click())

    # 其他操作

# 为了方便也可以使用代码中默认的弹窗监控逻辑
# 下面是目前内置的默认逻辑，可以加群at群主，增加新的逻辑，或者直接提pr
    # when("继续使用").click()
    # when("移入管控").when("取消").click()
    # when("^立即(下载|更新)").when("取消").click()
    # when("同意").click()
    # when("^(好的|确定)").click()
with d.watch_context(builtin=True) as ctx:
    # 在已有的基础上增加
    ctx.when("@tb:id/jview_view").when('//*[@content-desc="图片"]').click()

    # 其他脚本逻辑
```

另外一种写法

```python
ctx = d.watch_context()
ctx.when("设置").click()
ctx.wait_stable() # 等待界面不在有弹窗了

ctx.close()
```

### Watcher 监视器
**更推荐用WatchContext** 写法更简洁一些

~~You can register [watchers](http://developer.android.com/tools/help/uiautomator/UiWatcher.html) to perform some actions when a selector does not find a match.~~

2.0.0之前使用的是 uiautomator-jar库中提供的[Watcher]((http://developer.android.com/tools/help/uiautomator/UiWatcher.html)方法，但在实践中发现一旦uiautomator连接失败重启了，所有的watcher配置都是丢失，这肯定是无法接受的。

所以目前采用了后台运行了一个线程的方法(依赖threading库），然后每隔一段时间dump一次hierarchy，匹配到元素之后执行相应的操作。

用法举例

注册监控

```python
# 常用写法，注册匿名监控
d.watcher.when("安装").click()

# 注册名为ANR的监控，当出现ANR和Force Close时，点击Force Close
d.watcher("ANR").when(xpath="ANR").when("Force Close").click()

# 其他回调例子
d.watcher.when("抢红包").press("back")
d.watcher.when("//*[@text = 'Out of memory']").call(lambda d: d.shell('am force-stop com.im.qq'))

# 回调说明
def click_callback(d: u2.Device):
    d.xpath("确定").click() # 在回调中调用不会再次触发watcher

d.xpath("继续").click() # 使用d.xpath检查元素的时候，会触发watcher（目前最多触发5次）

# 开始后台监控
d.watcher.start()
```

监控操作

```python
# 移除ANR的监控
d.watcher.remove("ANR")

# 移除所有的监控
d.watcher.remove()

# 开始后台监控
d.watcher.start()
d.watcher.start(2.0) # 默认监控间隔2.0s

# 强制运行所有监控
d.watcher.run()

# 停止监控
d.watcher.stop()

# 停止并移除所有的监控，常用于初始化
d.watcher.reset()
```

另外文档还是有很多没有写，推荐直接去看源码[watcher.py](uiautomator2/watcher.py)

### Global settings 全局设置

```python
u2.HTTP_TIMEOUT = 60 # 默认值60s, http默认请求超时时间
```

其他的配置，目前已大部分集中到 `d.settings` 中，根据后期的需求配置可能会有增减。

```python
print(d.settings)
{'operation_delay': (0, 0),
 'operation_delay_methods': ['click', 'swipe'],
 'wait_timeout': 20.0}

# 配置点击前延时0.5s，点击后延时1s
d.settings['operation_delay'] = (.5, 1)

# 修改延迟生效的方法
# 其中 double_click, long_click 都对应click
d.settings['operation_delay_methods'] = ['click', 'swipe', 'drag', 'press']
d.settings['wait_timeout'] = 20.0 # 默认控件等待时间（原生操作，xpath插件的等待时间）

d.settings['max_depth'] = 50 # 默认50，限制dump_hierarchy返回的元素层级
```

对于随着版本升级，设置过期的配置时，会提示Deprecated，但是不会抛异常。

```bash
>>> d.settings['click_before_delay'] = 1  
[W 200514 14:55:59 settings:72] d.settings[click_before_delay] deprecated: Use operation_delay instead
```

**uiautomator恢复方式设置**

细心的你可能发现，实际上手机安装了两个APK，一个在前台可见（小黄车）。一个包名为`com.github.uiautomator.test`在后台不可见。这两个apk使用同一个证书签名的。
不可见的应用实际上是一个测试包，包含有所有的测试代码，核心的测试服务也是通过其启动的。
但是运行的时候，系统却需要那个小黄车一直在运行（在后台运行也可以）。一旦小黄车应用被杀，后台运行的测试服务也很快的会被杀掉。就算什么也不做，应用应用在后台，也会很快被系统回收掉。（这里希望高手指点一下，如何才能不依赖小黄车应用，感觉理论上是可以的，但是目前我还不会）。

~~让小黄车在后台运行有两种方式，一种启动应用后，放到后台（默认）。另外通过`am startservice`启动一个后台服务也行。~~

~~通过 `d.settings["uiautomator_runtest_app_background"] = True` 可以调整该行为。True代表启动应用，False代表启动服务。~~

UiAutomator中的超时设置(隐藏方法)

```python
>> d.jsonrpc.getConfigurator() 
{'actionAcknowledgmentTimeout': 500,
 'keyInjectionDelay': 0,
 'scrollAcknowledgmentTimeout': 200,
 'waitForIdleTimeout': 0,
 'waitForSelectorTimeout': 0}

>> d.jsonrpc.setConfigurator({"waitForIdleTimeout": 100})
{'actionAcknowledgmentTimeout': 500,
 'keyInjectionDelay': 0,
 'scrollAcknowledgmentTimeout': 200,
 'waitForIdleTimeout': 100,
 'waitForSelectorTimeout': 0}
```

为了防止客户端程序响应超时，`waitForIdleTimeout`和`waitForSelectorTimeout`目前已改为`0`

参考: [Google uiautomator Configurator](https://developer.android.com/reference/android/support/test/uiautomator/Configurator)

### Input method 输入法
这种方法通常用于不知道控件的情况下的输入。

```python
# 目前采用从剪贴板粘贴的方式输入
d.send_keys("你好123abcEFG")
d.send_keys("你好123abcEFG", clear=True)

d.clear_text() # 清除输入框所有内容

d.send_action() # 根据输入框的需求，自动执行回车、搜索等指令, Added in version 3.1
# 也可以指定发送的输入法action, eg: d.send_action("search") 支持 go, search, send, next, done, previous
```



```python
print(d.current_ime()) # 获取当前输入法ID

```

> 更多参考: [IME_ACTION_CODE](https://developer.android.com/reference/android/view/inputmethod/EditorInfo)

### Toast 提示
```python
print(d.last_toast) # 获取最后一条toast，如果没有toast则返回None
d.clear_toast()
```

> 在3.2.0版本中修复

### XPath 定位
Java uiautoamtor中默认是不支持xpath的，所以这里属于扩展的一个功能。速度不是这么的快。

例如：其中一个节点的内容（For example: 其中一个节点的内容）

```xml
<android.widget.TextView
  index="2"
  text="05:19"
  resource-id="com.netease.cloudmusic:id/qf"
  package="com.netease.cloudmusic"
  content-desc=""
  checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false"
  scrollable="false" long-clickable="false" password="false" selected="false" visible-to-user="true"
  bounds="[957,1602][1020,1636]" />
```

xpath定位和使用方法

有些属性的名字有修改需要注意

```
description -> content-desc
resourceId -> resource-id
```

常见用法

```python
# 等待10秒直到元素存在
d.xpath("//android.widget.TextView").wait(10.0)
# 查找并点击
d.xpath("//*[@content-desc='分享']").click()
# 检查是否存在
if d.xpath("//android.widget.TextView[contains(@text, 'Se')]").exists:
    print("存在")
# 获取所有文本视图的文本、属性和中心点
for elem in d.xpath("//android.widget.TextView").all():
    print("文本:", elem.text)
    # 字典示例: 
    # {'index': '1', 'text': '999+', 'resource-id': 'com.netease.cloudmusic:id/qb', 'package': 'com.netease.cloudmusic', 'content-desc': '', 'checkable': 'false', 'checked': 'false', 'clickable': 'false', 'enabled': 'true', 'focusable': 'false', 'focused': 'false','scrollable': 'false', 'long-clickable': 'false', 'password': 'false', 'selected': 'false', 'visible-to-user': 'true', 'bounds': '[661,1444][718,1478]'}
    print("属性:", elem.attrib)
    # 坐标示例: (100, 200)
    print("位置:", elem.center())
```

点击查看[其他XPath常见用法](XPATH_CN.md)

### Screenrecord 屏幕录制（已废弃 Deprecated）
视频录制(废弃)，使用[scrcpy](https://github.com/Genymobile/scrcpy)来代替吧

这里没有使用手机中自带的screenrecord命令，是通过获取手机图片合成视频的方法，所以需要安装一些其他的依赖，如imageio, imageio-ffmpeg, numpy等
因为有些依赖比较大，推荐使用镜像安装。直接运行下面的命令即可。

```bash
pip3 install -U "uiautomator2[image]" -i https://pypi.doubanio.com/simple
```

使用方法

```
d.screenrecord('output.mp4')

time.sleep(10)
# 或者做其他操作

d.screenrecord.stop() # 停止录制后，output.mp4文件才能打开
```

录制的时候也可以指定fps（当前是20），这个值是率低于minicap输出图片的速度，感觉已经很好了，不建议你修改。

# 启用uiautomator2日志 (Enable uiautomator2 logger)

```python
from uiautomator2 import enable_pretty_logging
enable_pretty_logging()
```

或者 (Or)

```
logger = logging.getLogger("uiautomator2")
# 设置日志器
```

## 停止UiAutomator (Stop UiAutomator)
Python程序退出了，UiAutomation就退出了。
不过也可以通过接口的方法停止服务

```python
d.stop_uiautomator()
```

## Google UiAutomator 2.0和1.x的区别
https://www.cnblogs.com/insist8089/p/6898181.html

- 新增接口：UiObject2、Until、By、BySelector
- 引入方式：2.0中，com.android.uiautomator.core.* 引入方式被废弃。改为android.support.test.uiautomator
- 构建系统：Maven 和/或 Ant（1.x）；Gradle（2.0）
- 产生的测试包的形式：从zip /jar（1.x） 到 apk（2.0）
- 在本地环境以adb命令运行UIAutomator测试，启动方式的差别：   
  adb shell uiautomator runtest UiTest.jar -c package.name.ClassName（1.x）
  adb shell am instrument -e class com.example.app.MyTest 
  com.example.app.test/android.support.test.runner.AndroidJUnitRunner（2.0）
- 能否使用Android服务及接口？ 1.x~不能；2.0~能。
- og输出？ 使用System.out.print输出流回显至执行端（1.x）； 输出至Logcat（2.0）
- 执行？测试用例无需继承于任何父类，方法名不限，使用注解 Annotation进行（2.0）;  需要继承UiAutomatorTestCase，测试方法需要以test开头(1.x) 


## 依赖项目 (Related projects)
- uiautomator jsonrpc server <https://github.com/openatx/android-uiautomator-server/>
- ~~uiautomator守护程序 <https://github.com/openatx/atx-agent>~~

# 贡献者 (Contributors)
- codeskyblue ([@codeskyblue][])
- Xiaocong He ([@xiaocong][])
- Yuanyuan Zou ([@yuanyuan][])
- Qian Jin ([@QianJin2013][])
- Xu Jingjie ([@xiscoxu][])
- Xia Mingyuan ([@mingyuan-xia][])
- Artem Iglikov, Google Inc. ([@artikz][])

[@codeskyblue]: https://github.com/codeskyblue
[@xiaocong]: https://github.com/xiaocong
[@yuanyuan]: https://github.com/yuanyuanzou
[@QianJin2013]: https://github.com/QianJin2013
[@xiscoxu]: https://github.com/xiscoxu
[@mingyuan-xia]: https://github.com/mingyuan-xia
[@artikz]: https://github.com/artikz

其他[贡献者](../../graphs/contributors)

## 其他优秀的项目 (Other excellent projects)
- https://github.com/atinfo/awesome-test-automation 所有优秀测试框架的集合，包罗万象
- [google/mobly](https://github.com/google/mobly) 谷歌内部的测试框架，虽然我不太懂，但是感觉很好用
- https://github.com/zhangzhao4444/Maxim 基于Uiautomator的monkey
- http://www.sikulix.com/ 基于图像识别的自动化测试框架，非常的老牌
- http://airtest.netease.com/ 本项目的前身，后来被网易广州团队接手并继续优化。实现有一个不错的IDE

排名有先后，欢迎补充

# 许可证 (LICENSE)
[MIT](LICENSE)


