---
title: Jmeter 用脚本处理器对数据进行处理
description: 用groovy脚本处理数据
date: 2021-03-04T16:06:29+08:00
tags:
  - Jmeter
  - 测试脚本
categories:
  - Test
image:
---

> 前两天在项目中写 jmeter 测试脚本，项目中唯一 ID 得调用端请求时生成并传送，于是查资料、翻官网文档，终于写出来了。

> 示例中我们使用简单的百度翻译的词典 API 请求 json 数据。

说明下，这里为啥要用 groovy 脚本，因为官网说 groovy 脚本会缓存编译结果，可提高性能。而且 groovy 脚本基本跟 Java 没啥区别

### 首先先创建个简单的 HTTP 请求

- 打开 jmeter 后，先在根节点(Test Plan)下新建个线程组(Thread Group)，默认的线程组配置就够我们本次示例用了
- 在线程组下新建个 Http 请求(HTTP Request)，再新建个结果树展示(View Results Tree)
- 配置 Http 请求，首先是协议(Protocol): `https` ，服务地址(Server Name): `fanyi.baidu.com` ，请求类型: `POST` ，Path: `/sug`
- 添加个请求参数(Parameters)，点击`Add`，Name: `kw`，Value: `test`
- 点击运行，或者使用快捷键`Ctrl+R`，运行结束在结果树中查看结果，看看是否请求成功

### 在发起请求前，填充数据

> 首先说说我们要达到的预期，我们要在请求参数中清理掉原本`kw`参数的`test`值，填充上新的参数值`success`

> 在写下面的东西前，先附上[官方文档](https://jmeter.apache.org/usermanual/component_reference.html#JSR223_PreProcessor)，这是 JSR233 前置处理器的文档。

- 先在 Http 请求节点下新建个 JSR223 前置处理器(JSR223 PreProcessor)，当然，也可以在同级节点建(不过必须要在请求节点后)，同级建的话，处理器的作用域就是同级节点的所有请求
- 选择脚本 `groovy`
- 编写脚本

  ```groovy
  // 获取请求参数
  def params = sampler.getArguments();
  // 获取第一个参数，这里排序跟请求节点的参数顺序挂钩
  def kw = params.getArgument(0);
  // 赋予kw新值
  kw.setValue("success");
  ```

- 运行测试下请求参数是否已经更改

写到这里，我真的忍不住想吐槽下，这请求参数藏的够深的啊 😑 官方文档也没写这点，要不是看 API 文档，还真就不一定发现的了 Arguments。官方文档里 `Parameters`、`args`、`ctx`、`props`都试了，活生生让我试出来的啊 😤！！！

### 在请求结束后，对返回的数据进行格式化处理

> 返回数据的处理，主要是对返回的数据进行解码、Json 格式的缩进展示

> 后置处理就方便的多了，在 JSR223 后置处理器(JSR223 PostProcessor)中，有`prev`参数可直接操作结果集

- 同前置处理器一样，先建个 JSR223 后置处理器节点，作用域同前置处理器
- 同样，脚本选择`groovy`
- 编写脚本

  ```groovy
  import groovy.json.JsonOutput;
  import org.apache.commons.lang3.StringEscapeUtils;
  // 字符串的形式读取返回数据
  def data = prev.getResponseDataAsString();
  // 格式化json数据
  def json = JsonOutput.prettyPrint(data);
  // 对数据进行Unicode转中文的解码
  def ret = StringEscapeUtils.unescapeJava(json);
  // 把我们转换后的数据设置为返回结果，编码设置为UTF-8
  prev.setResponseData(ret, "UTF-8");
  ```

- 运行测试结果，完美结束！

至此整个前置后置的处理就完美了
