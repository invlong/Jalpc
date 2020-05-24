---
layout: post  
title: "Httprunner使用教程"  
date: 2020-05-23 14:27:50 +0800  
categories: Java培训  
tag: "工具"  
---

### 一.使用charles录制

导出格式要使用har格式。

### 二.使用hartocase将har文件转换为yml格式文件

hartocase是httprunner的一个核心工具之一，作用是将通过charles录制的har文件，转换成httprunner可以使用的测试用例。字面意思也很直白，har转换成测试用例（case）。

```shell
# -2y 意思是to yaml，即.yml格式的文件
har2case docs/data/demo-quickstart.har -2y
```

通过上面的领命，会在har文件的同级目录下，生成一个同名的.yml文件。

### 三.使用hrun命令执行yml文件，开始执行测试脚本

```shell
# --log-level debug 是打开debug模式，可选
hrun docs/data/demo-quickstart-0.yml --log-level debug
```

执行上面命令后在同级的reports目录下，有一个测试报告，会详细同级执行结果，是个html文件，双击打开即可查看结果。

### 四.遇到的问题

#### 1.通过hrun执行测试用例的options请求返回404

分析：由于服务器对请求的来源做了限定。

解决：加上header：

`-H 'Origin: http://wxk12test.12kcool.com'`

#### 2.登录请求成功了，并且校验通过了，但是最后确实error返回

错误信息如下：

The problem is that `"{}"` is non-Unicode `str`, and you're trying to `format` a `unicode` into it. Python 2.x handles that by automatically encoding the `unicode` with `sys.getdefaultencoding()`, which is usually `'ascii'`, but you have some non-ASCII characters.

分析：这个问题是由于Python2.x对中文编码有bug，只能升级Python到3.6之后版本才可以，并且一旦安装了httprunner，没法升级Python2.x到3.x，因此上篇建议，Python使用pyenv安装多环境以及使用虚拟环境。