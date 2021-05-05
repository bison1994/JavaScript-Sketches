### 测试的分类

- 根据是否关注内部逻辑
  + 黑盒测试：不关注程序内部逻辑，重在穷举各类输入是否返回预期输出
  + 白盒测试：关注程序内部逻辑，重在 API 与逻辑链路的测试
- 根据测试对象的范围
  + 单元测试：针对单个功能点的测试
  + 集成测试：在特定环境下对多个组件、甚至多个系统的测试
  + 端到端测试：模拟用户操作，涵盖客户端到服务端交互的测试
- 其它类型
  + 渗透测试：通过模拟恶意攻击进行安全性测试


### 前端的测试

- GUI 产品不太适合 API 测试或自动化测试，因为经常变动，使得 case 的维护成本很高，而且 case 写起来很麻烦。因此通常仍然需要人肉测试
- 一般只有最稳定的通用组件、通用方法库、底层框架比较适合 API 测试
- 在项目维护和迭代过程中，确定回归范围是一个很重要的环节，这部分有自动化实现的方案
- 前端自动化测试需要考虑到的点
  + 各类目标浏览器兼容性测试 => 并行测试 => 云测试
  + 迭代更新后的回归范围与测试
  + 纯 js 组件/库/框架的测试
  + UI 组件的测试
  + 测试的覆盖率
  + 测试用例的编写、维护及其成本
  + 自动化测试与持续集成工具的配合

> [参考](https://www.zhihu.com/question/29922082)


### 前端测试工具

- 测试环境
   + Selenium、Appnium、phantomjs 
- 测试框架
  + Unit test 框架：Karma、Mocha、Jasmine、Jest
  + e2e test 框架：Nightwatch
- 断言库
  + chai、should.js
- 测试覆盖率
  + Istanbul


### Selenium

- Selenium 是一个自动化测试工具集，其核心功能是驱动浏览器模拟执行各种用户操作（Selenium automates browsers）。它提供了统一的接口，使得各种语言都可以调用其接口来定制测试用例。
- Selenium 主要针对 PC 端，移动端类似的工具集是 Appium
- Selenium 工具集的组成
  + Selenium 2 (aka. Selenium WebDriver)
  + Selenium 1 (aka. Selenium RC or Remote Control)
  + Selenium IDE (Integrated Development Environment)，一个 firefox 插件，无需使用编程语言
  + Selenium-Grid 用于并行测试
- 推荐的用法是：特定语言的 Selenium WebDriver + 特定浏览器的 driver
- 基本用法
  + 安装 python3.6+
  + 安装 python 版的 selenium。命令：`pip install selenium`
  + 安装特定浏览器的驱动（driver）。
    - 以 Chrome 为例，[下载地址](https://sites.google.com/a/chromium.org/chromedriver/downloads)
  + 配置环境变量。
    - 以 windows10 为例：控制面板 => 系统 => 高级系统设置 => 环境变量 => 用户变量 => 选中 path => 编辑 => 新建 => 添加 chromedriver.exe 所在的路径 => 确定
    - 新打开一个命令行，运行 `chromedriver`，如果正常运行，说明配置成功
  + 编写 python 代码，保存在 test.py 中

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
driver = webdriver.Chrome()
driver.get("http://www.baidu.com")
elem = driver.find_element_by_id("kw")
elem.send_keys('aaa')
elem.send_keys(Keys.ENTER)
```
  + 运行 `python test.py`，如果 Chrome 浏览器自动打开，并且选中输入框，输入了相应内容，并跳转到搜索结果页，则说明运行成功

> [参考教程](http://selenium-python.readthedocs.io/installation.html)

### Resource

- [a-guide-to-automating](https://codeburst.io/a-guide-to-automating-scraping-the-web-with-javascript-chrome-puppeteer-node-js-b18efb9e9921)
- [Automatic visual diffing with Puppeteer](https://meowni.ca/posts/2017-puppeteer-tests/)
