# 知网爬虫
因为课程项目需要用到知网爬虫，在GitHub上搜索后发现[CyrusRenty的CNKI-download](https://github.com/CyrusRenty/CNKI-download)仓库，但是clone下来后发现不太能用，因此对其进行了修改，除下载文献以及验证码功能外，其余功能亲测可用。

因为仅接入校园网不能使得知网识别出学校（需要通过另一个学校VPN才行），下载文献功能我无法使用o(╥﹏╥)o，也就没能修改。验证码我也没有遇到，在爬取每个页面时尽量保证足够的间隔时间应该可以规避该问题。

PS：对于NoneType等问题，我粗暴的try catch忽略了，因此如果您看到爬取完成后的Excel中某行有的字段没有数据，当做噪声删掉就好~如果您迫切需要所有完整字段，提issue我看看能不能帮到

PS：有时运行爬虫时可能会报以下错误：

[![TO8ly6.png](https://s4.ax1x.com/2022/01/04/TO8ly6.png)](https://imgtu.com/i/TO8ly6)

这种情况，请关闭全局代理的VPN，并在浏览器中输入中国知网地址查看能够正常进入，之后反复尝试运行程序即可。

关于本项目如果您有问题，可以直接提issue，希望我能在力所能及的情况下帮助您！如果您有更好的代码合入，肥肠欢迎提PR！

接下来的内容取自原作者，向原作者致敬∠(°ゝ°)

---

项目是基于Python3 实现的爬取知网数据的爬虫，可根据知网高级检索进行搜索，提供文献基本信息、文献下载、文献摘要等详细信息爬取功能。

实现过程可以查看[我的博客](https://cyrusrenty.github.io//2018/12/19/cnkispider-1/)

程序运行如下：

![](https://i.imgur.com/0P9erW1.jpg)

详细信息excel表格如下：

![](https://i.imgur.com/3qgNkSa.png)

下载caj如下：

![](https://i.imgur.com/zYACe4A.png)

# 特点
* 通过发送解析包形式抓取数据，相比于使用selenium等方式性能稍高一些。
* 可使用知网高级检索功能进行搜索，更高效检索文献。
* 可根据网络及知网反爬虫情况选择性开启详细信息抓取及下载caj文献功能。
* 利用excel表格快速查看所需文献摘要等信息，可根据excel提供下载链接选择性下载，防止下载过快导致知网反爬。

# 使用方法
## 安装依赖
>在验证码处理部分使用了`tesserocr`，不过验证效果目前不是很好，所以默认开启手动识别验证码。
>
>如果本地没有安装`tesseract`，可以先安装这个，再执行`pip install tesserocr`。或者将`CrackVerifyCode.py`文件第15、63、64行注释后再执行安装命令。

```shell
pip install -r requirements.txt
```


## 配置选项


```shell
# Config.ini 为项目配置文件
# 0为关闭 1为开启
isDownloadFile = 1     # 是否下载文件
isCrackCode=0          # 是否自动识别验证码
isDetailPage=0         # 是否保存文献详细信息到excel
isDownLoadLink         # 是否在excel中保存下载链接
stepWaitTime=5         # 每次下载及爬取详情页面停顿时间
```

建议下载和爬取详情页面不要同时开启，停顿时间不低于3秒。

## 启动程序

```shell
python run-spider.py
```

## 运行后文件结构说明
爬虫运行完毕后，所有数据将保存在data文件夹下，data文件夹每次重新运行程序会自动删除旧的。
```
CNKI_download
  -- data                        存放所有爬取数据
       -- CAJs                   存放所有下载的caj原文
            -- xxxxxxx.caj
            -- xxxxxxx.caj   
       -- Links.txt              所有爬取文献的下载链接
       -- ReferenceList.txt      爬取文献简要信息
       -- Reference_detail.xls   文献详细信息excel表
```

## 注意事项
* 项目运行的前提是电脑可以通过ip访问知网并下载（一般学校都买了数据库），快写完时候发现目前还有一个跳转接口，后续后增加公网访问。
* 如果出现“远程主机拒绝了访问”可以适当加长每次停顿的时间。
* 如果在运行过一次后，再次运行前记得关闭data文件夹中所有文件，否则可能会由于无法删除data文件夹报错。
* 如果只爬取信息不下载的话，可能会在运行1000条文献左右出现反复输入验证码情况（即使输入正确）。目前还不知道是什么原因

# TO DO LIST
* 完成高级检索的其他未实现功能。
* 增加指定开始爬取页面信息，实现从上次错误处再次爬取。
* 增加公网跳转至知网接口，保证无法IP登录用户也可使用本爬虫。
* 创建代理池，基于公网跳转实现代理ip访问，减少知网封ip及输入验证码次数。
* 撰写程序实现及分析过程记录。
