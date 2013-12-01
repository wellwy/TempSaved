##目标

旧版滔滔导出。

##思路

简单的看了下js里的数据。发现他会把数据分页的load到js中，然后缓存起来，分页渲染。那么如果想导出这部分数据，就可以直接把js里的数据打印出来即可。

##问题

旧版滔滔[地址][http://ctc.qzs.qq.com/qzone/blog/blog_mood.html].

目前旧版滔滔有bug，最多只能展示148 个（貌似非必现。看了下代码，获取总量的cgi返回的数据有误。）

如下：

    http://e.qzone.qq.com/cgi-bin/cgi_emotion_indexcount.cgi?uin=XXXX&r=0.5181506380904466

因此导致在计算页数时，计算错误。无法看到后面的页数。而js开放的接口没有几个，在不修改js文件的前提下，能做的操作相当之少。

然后看到了另外一个cgi：

    http://e.qzone.qq.com/cgi-bin/cgi_emotion_indexlist.cgi?uin=XXXX&emotionarchive=-1

这个cgi最后有个参数`emotionarchive=-1` ，这个`emotionarchive`的值代表页数，从1~n，如果为-1，则显示最后一页的数据。

##方法

###Step0：准备
- 登录自己的QZone
- 访问旧版滔滔[地址][http://ctc.qzs.qq.com/qzone/blog/blog_mood.html].

###Step1：确定总条数
- 在浏览器中贴上获取列表的CGI

    http://e.qzone.qq.com/cgi-bin/cgi_emotion_indexlist.cgi?uin=XXXX&emotionarchive=-1

- 记得把uin的参数换成刚才登录的QQ号。
- 看返回的结果，记下自己的`rss->channel->item[].id`的最大值。

###Step2：修改总条数的Bug
- 打开浏览器的控制台。
- 输入`TT.data.total = xxx`，xxx为刚才的记下来的id的最大值。

###Step3：加载数据

由于很多方法不能直接调用，所以加载数据的最好的办法就是直接点下一页，一路点下去。但是如果页数比较多，就比较恶心了。。所以这里写了一个循环。直接调用下一页的函数即可。

同样在控制台输入以下代码

    for(var n = 0;n < Math.ceil(TT.data.total/6);n++){
        setTimeout(TT.public.go(true),500);
    }

ps：如果中途报错，可能是频率限制导致的，点确定后继续。可以无视。最后如果因为中途出错，有几页没有翻完，手工点下翻页即可。

###Step4：导出数据

现在数据都已经加载到了内存中，只需要导出即可。这里用了最简单的办法。直接在控制台输出。代码如下

    for(var i = 0 ; i < TT.data.total ;i++)
    {
        if(TT.data.list[i]){
        console.log(TT.data.list[i]._id +"\t"+TT.data.list[i].expressionTitle+"\t"+TT.data.list[i].pubTimeString+"\t"+TT.data.list[i].title.value);
        }
    }

把内容copy出来，因为用`\t`分隔，因此可以直接贴到excel里。Enjoy It！

##后记

至于评论。没有找到相应的cgi。暂时无解了。。


