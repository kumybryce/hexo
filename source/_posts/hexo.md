---
title: hexo 博客创建、部署、美化过程记录
date: 2020-4-20 21:00
updated: 2020-4-20 21:00
tags: 
  - hexo
categories:
  - 博客
aplayer: true
---

##  前言
作为一名信息工程专业的学生，编程语言没学多少，正规讲过的且了解比较深的是`C++`，也只是一些基本的编程，连`数据库`、`数据结构`还有各种算法都没学过，`python`、`java`这些就更不用说了，`HTML`语言、`CSS`、`JavaScript`更是只听说过了。当然，我们还学过一点点`汇编`，非常浅显地学习过`HDL`语言，毕竟是个工科生，软硬都得沾点边，当然，`matlab`也是学过的，这是科研神器。但是，无论是项目中还是比赛中又或者是个人需要，都可能会用到上面没有学过的语言，上学期的`计算机图形学`更是直接让我们用`JS`和`HTML`编写`WEBGL`，这对于完全没学过这两门语言的人来讲简直是灾难，编程全靠看历程猜语法，结果只能是借助第三方库。。。又比如`机器学习`，`python`自然必不可少，网站前端又得使用更多的动态语言，所以要想做自己想做的东西，就得不停利用闲暇时间自学这些语言，有些会系统地学，有些因为赶时间只是临时查一下。好了，废话说的太多了，这篇文章就是记录一下自己搭建`hexo博客`过程中遇到的困难和解决的措施以及待解决的问题，旨在记录，以免回头连自己都忘了，当然也希望有像我一样背景的，想要搭建hexo博客得小白，能从这篇文章中得到一些帮助，更希望专业的朋友能够给我宝贵的意见。
## hexo介绍
`hexo`是用于搭建个人博客的，就像`wordpress`那样，不过`wordpress`更适合做网站，比较臃肿，`wordpress`的插件也是相当丰富，非常适合小白建站。`hexo`是轻量化的博客搭建工具，它可以以静态页面形式部署在代码托管网站上，比如`GitHub`、`Gitee`、`Coding`等，`hexo`社区也有很多别人开发好的主题以及各种插件，这些代码都是开源的，可以在`GitHub`上获取，并且他们的`readme`都写得十分详细，甚至有些还有专门的文档，对于小白来说如果只想实现插件有的功能，按照文档走完全没有问题。
## 我的环境
说到博客，环境说明是很重要的，大家可能会遇到这种情况，按照别人的博客教程走，结果一大堆错误，这也是让我感到十分痛苦的经历，有一句话说得好**没有介绍环境的教程都是放屁**，我搭建的时候是按照[这篇博客](https://www.jianshu.com/p/84ae2ba1c133)走的，基于`码云`（码云类似于`GitHub`，是一个代码托管网站，国内的版本，用法和`GitHub`基本一样，不过因为在国内，所以访问速度快得多），如果还没有搭建的话跟着这篇博客走一遍就好了，到后面我发现码云的页面发布是手动的，也就是说每次提交代码上去都要手动更新，当然它也不是不支持自动部署，但是要钱，对于我这种穷屌丝，emmmm，宁愿手动，但是后来因为`百度收录`的问题，我不得不转战其它托管网站，`GitHub`我就不说了，`Coding`也是一个国内的，非常好用的代码托管网站，**5人以下的项目是免费的**（对于个人博客，开发者当然一般只有你一个人），界面也是我喜欢的，关键是它可以自动部署，可以自定义域名（前提是你有自己的域名），这在码云上可都是收费内容啊。具体怎么迁移我后面会讲到，其实只要你在码云上部署的流程走完了，你也就明白这整个原理了，迁移到`coding`上也就分分钟的事。讲到这里，还没说我的环境，其实我已经交代的差不多了，因为是跟着上面那个博客走的吗，我的环境罗列出来就是：`Windows10`，`hexo`，`node.js`，`git`，`gitee`，`coding`，`Chrome 80.0.3987.163`，hexo使用的主题是`hexo-theme-yun-dev`，这里要强调一下，这个主题我觉得开发者非常用心，特别是文档，写得十分详细，界面也好看，配置项特别多（配置项越多越好啊，说明更多可控因素可以被你傻瓜式控制，比如各种颜色、各种图标、各种插件），出于敬意，这里贴出此主题的[GitHub主页](https://github.com/YunYouJun/hexo-theme-yun)以及它的[示例站点](https://www.yunyoujun.cn/)，当然，你也可以参观一下[我的站点](http://www.kumybryce.work)（不要脸的求收藏）,这里顺便贴出在解决问题过程中发现的做的非常好的站点：[Achirou](https://www.jsonpop.cn/)（个人觉得这个人超厉害）Achirou网站部署在阿里云上的（服务器要钱）静态文件加速和pjax动态替换让其网站访问体验极佳！
## 我使用hexo的大致流程（没啥参考的）
因为我的博客主要记录一下编程过程中遇到的问题，所以最常用的就是发布文章，当然随着学习的越加深入，我也会开始在站点上记录个人生活的点滴，相册啊啥的，到写这篇文章(2020/4/20)为止，博客上只写了几篇编程的博客，因为刚开始嘛嘻嘻。进入正题，我的流程就是：
1.  CSDN编辑文章

>在线编辑markdown嘛，方便快捷，其实它还有很多好处，就是CSDN为你提供了图床，在其他地方发布只需要把文章导出为markdown就行，文章中的图片不会失效，当然你也可以自己做图床，本地编辑，具体就百度吧哈哈哈哈
2. 小书匠本地调整
>其实就是需要简单修改一下文字布局啥的时候本地的markdown编辑器，做的挺好的，还可以切换主题，自定义图床等，但我懒（其实是之前在码云和GitHub上尝试过，只是失败了哈哈哈哈），所以不用这个编辑器插入图片
3. `markdown`文件放到hexo本地文件系统的`post`中（废话），`hexo g -d`推送
4. 码云手动部署，当然coding上就不用了

## 迁移Coding
迁移到`coding`上其实很简单，按照上面的步骤，在`Coding`上建立仓库和项目，然后在`项目设置->功能开关`中开启`持续部署`和`持续集成`功能：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200420205534660.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70)
然后在项目中就可以持续部署为`静态网站`了：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200420205741985.png)
新建一个网站，就部署起来了，点击网站中的设置可以更改域名，将域名以CNAME方式解析到这个网站，然后再在`Coding`中增加域名，可以增加多个。


## 修改主题(持续更新)
博客这个坑之深是我等初级程序猿所无法想象的，但这是一个很好的展示平台，可以借此机会学习很多很多实用的前端知识！修改主题这一部分除了主题的文档，也就是这个主题集成的功能外想要加其他的功能或者修改一些小效果就需要一些编程知识了，我也是临时摸打总结的一点经验，后面会系统的去学习这些语言，慢慢更新吧。
### 文件结构
先展示一下我的文件结构，介绍主要文件的作用，这对于修改主题必然是会有很大帮助的，当然，不一样的主题文件结构会不同，用的语言母版也有所不同，但文件结构大同小异，你只要知道有哪些必要的文件，对应于你的主题就好找多了
>#### `主文件夹结构`![主文件结构](https://img-blog.csdnimg.cn/20200420145628956.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70)
> 这是主文件结构，`hexo`是我的博客本地文件，我单独放在了`E盘`，hexo便是在这里初始化的，每次本地调试和推送的时候也是在这个文件夹内`git bash here`的，这个文件夹下面的文件我介绍一下，`live2d_models`是我添加的一个看板娘插件需要的，所以你们一开始是没有这个文件夹的，`.vs`是`visual studio`生成的，因为我使用`VS`来编辑代码的，很好用，特别是写`css`的时候，颜色啥的直接可以弹出色板，选颜色太方便了，然后其他的文件夹应该都一样，其中重要的是`_config.yml`，这是主配置文件，主题也有自己的配置文件，就叫它辅配置文件吧哈哈，后面再介绍。`package.json`里面记录了你安装的第三方插件和一些工具，`source`是你的文章、标签、目录等内容，`themes`是主题文件，各种资源（如图片，css，js等）都是在这个里面的，`public`是发布文件夹，最后推送就是把这个文件夹的东西推到远端，而`hexo g`其实就是根据主题文件、配置文件等把一个网站根目录需要文件生成到`public`文件夹中所以改`public`中的东西是没有用的，`hexo clean`就是把`public`文件夹删除掉啦，当然还有删除`db.json`之类的。然后看看子文件夹:

>#### `source文件夹`
>![source文件夹](https://img-blog.csdnimg.cn/20200420151703218.png)
`_posts`是每次发布的文章放的地方，`about`是博客中关于你的页面，其中`site.md`是关于站点的文件，其它的主要都是`index.md`，只需要初始化一下，具体就百度吧，很简单的，要引入这些使其生效都要`npm install`插件的，各主题配置的时候文档里应该会写。

>#### `theme文件`
>![theme文件夹](https://img-blog.csdnimg.cn/20200420152326786.png)
这个文件夹中重要的是`_config.yml`、`layout`文件夹、`source`文件夹，`_config.yml`中配置你的主题，这不用我说了，就是辅配置文件，`layout`是布局文件，要改博客各个页面的样式和内容都在这个里面，比如主页的侧边栏你要加一张萌萌哒的动图，那么就需要找到`layout->_patial->sidebar.pug`更改即可，注意，这个主题使用的`html`生成模板语言是`pug`，其它主题可能是`.ejs`或者是其它的，其实就是一种生合成`html`的模板语言，要改又不会语言的百度学一下就好了，我这里用`pug`的时候找到[一个网站](https://pughtml.com/)，可以转化pug和html语言，为什么要用模板生成`html`，其实主要是`html`语法结构导致要书写很多不必要的东西，比如`</script>`这种，所以就有了这些语言（只是个人推测和感觉，具体的还是要自己去查一下，我没学过`html`也没学过`pug`所以参考价值不大），你如果看到别人的博客或者某个页面的某个部件很好，就可以调出浏览器开发者工具来扒一下`html`源码，然后放到上面那个网站转换一下语言，再粘贴到对应的文件里面就可以了，注意，上述网站经过我的测试，在`chrome`中不兼容，在`firefox`中可以使用，很多在线工具如果点着不响应很有可能就是浏览器不兼容，`firedfox`做的还是很不错的。`source`文件夹是放资源的，最终会生成到`public`文件夹下，所以在调用资源的时候比如`source->img`下有一张图片，调用的相对路径就是`/img/**.jpg`，`css`文件夹是更改css要用到的，这个比较重要，还是一样，要改那个css就去这个文件夹里面找。

### 调试环境
hexo提供本地调试服务，`hexo s` 就可以开启，只要这个服务开着，你就可以在本地输入`http://localhost:4000/`访问网站，然后你修改pug文件和css文件不需要重新生成，它不和`public`挂钩，改了文件看一下`git bash`没出错就可以刷新浏览器页面看看你效果了，我使用的是`chrome`，在`更多工具->开发者工具`打开控制台。查看是否报错或者`html`和`css`是否符合预期。本地测试通过后再`hexo g -d`推送就可以了。
### 记录我修改过的东西以及踩过的坑
#### 阿里云[iconfont](https://www.iconfont.cn/)图标引用
这个功能非常nice，之前用阿里云图标都是在开发软件或者做PPT、海报的时候需要用到图标在这个上面下载的，那个时候就觉得阿里云图标这个平台是在是太好了，因为好多图标网站收费图标资源还丑，iconfont完全免费，真有种高质量开源社区的感觉，所以自己上传图标都不好意思上传丑的。接下来就介绍怎么使用吧，其实[官方文档](https://www.iconfont.cn/help/index?spm=a313x.7781069.1998910419.13)写的非常详细，我就写写我的使用过程吧。
>- iconfont端
>创建自己的项目
>![iconfont](https://img-blog.csdnimg.cn/20200420161822204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTY3MjgyOA==,size_16,color_FFFFFF,t_70)
点击新建项目（具体位置聪明的你自己找，就在这个页面中），然后就可以搜素你想要的图标，找到后加入购物车（我也不知道为什么是这个符号），可以选很多，选完之后点击右上角的购物车，加入到你刚才创建的项目中，然后再回到上述页面，然后选择`symbol`，其它的引用方法自行查看官方文档。选择生成项目链接，就会生成一个`cdn`资源链接，关于`cdn`后面再简单介绍，具体的我也不懂。然后项目下面就会有你选的图标，然后点击复制代码就行，这个代码就可以在网中使用了，每次更新项目图标都要重新生成链接，替换刚才的链接，所以还挺麻烦。记住这个链接和图标代码
>- 博客配置端
>把资源链接引入到你的页面里面，我是直接在配置文件的`head`项加入的，如下图
![iconfont](https://img-blog.csdnimg.cn/2020042016285571.png)
其实就是在`head`标签中引入了这个`script`如果你的主题没有引入自己的`js cdn`资源配置项的话，就在生成head的模板文件中之家引入这个链接就好了，具体引入的程序根据模板文件来，不会的可以留言交流哦，是否引入成功可以在你的网站页面打开开发者工具看看`<head>`标签下是否引入。我是用这个主题支持自定义链接和联系方式啥的，还可以定义其图标，所以使用刚才的代码如下图所示
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200420163947415.png)
好了，`iconfon`就记录到这里啦
#### [aplayer](https://github.com/MoePlayer/APlayer)音乐播放器（目前还有很多问题没有得到解决）
`aplayer`播放器可以说非常舒服了，这里记录一下使用过程。其实这个主题支持`aplayer`，文档也有写，但是不全，很简略。首先要下载这个插件
>num install hexo-tag-aplaye
	
然后简单的来说就四行语句（pug）


```javascript
link(rel='stylesheet', href='https://cdn.jsdelivr.net/gh/kumybryce/resource@v1.107/mycdn/css/APlayer.min.css'
script(src='https://cdn.jsdelivr.net/npm/aplayer@latest/dist/APlayer.min.js')
script(src='https://cdn.jsdelivr.net/npm/meting@latest/dist/Meting.min.js')
meting-js#4976294412(server='netease', type='playlist',fixed='true', list-folded='true', autoplay='false', volume='0.4', theme='#4fd0d8', order='random', loop='all', preload='auto', mutex='true')
```

前三个是引入`css、aplayer.js、meting.js`，什么是meting呢，它是各大音乐平台支持的一种脚本啦，具体百度吧，着重讲第四句

| 选项  | 描述  |
| -- | -- |
| meting-js#4976294412 | 这个就是说使用meting来加载你的音乐，歌单号是4976294412 |
| server | 音乐平台: `netease`, `tencent`, `kugou`, `xiami`, `baidu` |
| type | `song`, `playlist`, `album`, `search`, `artist` |
| fixed | 开启固定模式 |
| mini | 开启迷你模式 |
| loop | 列表循环模式：`all`, `one`,`none` |
| order | 列表播放模式： `list`, `random` |
| volume |播放器音量 |
| lrctype | 歌词格式类型 |
| listfolded | 指定音乐播放列表是否折叠 |
| storagename | LocalStorage 中存储播放器设定的键名 |
| autoplay | 自动播放，移动端浏览器暂时不支持此功能 |
| mutex | 该选项开启时，如果同页面有其他 aplayer 播放，该播放器会暂停 |
| listmaxheight | 播放列表的最大长度 |
| preload | 音乐文件预载入模式，可选项： `none`, `metadata`, `auto` |
| theme | 播放器风格色彩设置 |

没有写入的项按照默认方式给出，具体可以参考[官网文档](https://aplayer.js.org/#/zh-Hans/?id=%E5%8F%82%E6%95%B0)。其实你把上面四句话按照自己需求改一下，然后转换成你的模板语言，最后在页面收纳柜生成html就可以了，然后如果要改样式，就需要更改`css`，因为是`cdn`引入的资源，所以你需要把这个`css`文件下载下来编辑好再引入，还是建议用`cdn`引入，这样一来你就需要构建自己的`cdn`了，下面讲讲构建自己的`cdn`。
我是在`GitHub`上存放我的资源的，在`GitHub`中新建仓库，然后在本地初始化，把修改后的`css`文件放进去（任何资源都可以），`push`到`GitHub`上，这些操作很基础，不懂的可以学学`git`，这里有一篇[教程](https://www.liaoxuefeng.com/wiki/896043488029600/896067008724000) 。
好了，现在我的博客终于有音乐播放器了，效果如下图：
>![aplayer](https://img-blog.csdnimg.cn/2020042017260380.gif)
>什么？你的做出来没有吸附边缘？一开始播放列表就打开了？切换页面音乐停止？好叭，这都是我遇到的坑

>首先，怎么做吸附功能，其实在[这篇博客](https://www.jsonpop.cn/posts/19777cfa/)中记录了怎么做吸底模式，但我失败了，所以直接用了上面的代码来搞，原理是一样的，只是他的方法是把参数接口单独拿出来放在配置文件里面了，以方便他人。那么那篇博客中的音乐播放器为什么吸附了呢？我思考了一下，最终的想法是找到播放器的标签，再在播放器的css文件中设置当鼠标悬停在箭头标签上时，将播放器的左偏移量设置为0，其它时候左偏移为负值，除了箭头那一块其它部分全部偏出屏幕，这个功能就实现了。说着简单啊，为了实现这个我查了好多资料，真是一个外行人的痛苦。更恐怖的是，当我完成之后无意间发现在那片博客的评论区就有答案，真的是蠢到家了，这告诫我们一定要把每一个参考认真读完！至于如果你想要用css控制各个元素的形态，只需要在开发者工具中找到此时对应标签所属的类，然后在其`css`文件中定义这个类的各种动作，当然，也可以直接在`html`中嵌入`style`，但不推荐。

>然后就是一开始列表就打开，这里解决办法是将配置项中`mini:false`去掉

>再说说切换音乐停止吧，这个问题我还没解决，每次点击链接页面都会刷新，也就意味着所有资源重新加载，音乐自然就没了，如果要让页面不重新加载，或者局部刷新，就需要pjax局部替换，这个太高级了，可以大大优化网页浏览体验，应该是先获取链接内容，然后根据html标签内容替换，可以定义哪些是局部替换的，这样就可以实现无刷新浏览了，页面切换速度也是快到飞起。如果你看到谁的博客点击文章时浏览器标签没有转圈，那么它就使用了这个技术了，目前我还遇到肯多困难，无法实现局部刷新，后面再说吧。
#### 看板娘！敲黑板
看板娘就是一个可爱的模型在你的页面中，它有各种动作，还会一直看着你的鼠标，点它还会互动之类的，现在官方的模型有一些动漫人物和动物（死宅是不是已经露出了憨憨笑？）先展示下效果吧，或者去我的博客参观。
<img src="https://img-blog.csdnimg.cn/20200420193704587.gif" atl="看板娘" height="300">
这个就是live2D插件啦，[官网在此](https://www.npmjs.com/package/hexo-helper-live2d)！先下载安装上：
>num install hexo-helper-live2d

再下载[模型](https://github.com/xiazeyu/live2d-widget-models)，可以先[预览一下](https://huaji8.top/post/live2d-plugin-2.0/)，比如我是用的是`live2d-widget-model-shizuku`则
>npm install live2d-widget-model-shizuku

完成后再博客根目录新建文件夹`live2d_models`，将`node_moduels`中的`live2d-widget-model-shizuku`复制到刚新建的文件夹中，很多博客也没有这一步，我没有试验，大家可以自己尝试一下。
然后在主配置文件中加入配置语句：
```yaml
# Live2D
## https://github.com/xiazeyu/live2d-widget.js
## https://l2dwidget.js.org/docs/class/src/index.js~L2Dwidget.html#instance-method-init
live2d:
  model:
    scale: 1
    hHeadPos: 0.5
    vHeadPos: 0.618
    use: live2d-widget-model-tororo // 下载的动画模型名称
  display:
    superSample: 2
    width: 120
    height: 200
    position: left // 模型在网页显示位置
    hOffset: 20
    vOffset: 50
  mobile:
    show: true  // 移动设备是否显示
    scale: 0.5
  react:
    opacityDefault: 0.7
    opacityOnHover: 0.2
```
注意，这个时候`hexo d -d`应该就可以看到了，反正我失败了，解决办法是在配置文件夹中加入：

```yaml
plugins
  - hexo-helper-live2d
```
这时候又会有新问题，你会发现你连`hexo g`都不行了，报错是没有这个指令，在`package.json`中明明有这些包啊，左思右想发现是加入上面一句话的意思就是只使用这一个插件，`server、git`啥的都是插件，所以不得不把`package.json`中所有的包都写入配置文件中，这就很麻烦了，只要有新插件，就必须引入一下。。。希望你们不要遇到这种问题。
#### 头像旋转
其实这没啥说的，学过css的肯定觉得我在这个说这个简直就是在放屁一样，没办法，咱没学过呀，所以记录一下咯，找到`css->_components->sidebar->site-overview.styl`在头像的css里头像img下加上下面语句：

```css
-webkit-transition: -webkit-transform 1.0s ease-out;
-moz-transition: -moz-transform 1.0s ease-out;
transition: transform 1.0s ease-out !important;
```
在`img:hover`下加上：

```css
-webkit-transform: rotateZ(360deg);
-moz-transform: rotateZ(360deg);
transform: rotateZ(360deg);
```






## 参考博客：

>[aplayer](https://www.jsonpop.cn/posts/19777cfa/)
[看板娘](https://sevencho.github.io/archives/cb206c67.html)
[头像旋转](https://blog.csdn.net/qq_43020645/article/details/82793753)

## 未解决问题
1. aplayer无法全局播放
2. aplayer加入后目录无法使用