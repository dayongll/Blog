# 一个没有任何工程经验的人眼中的前端工程化

## 背景

*本文的作者没有任何大型前端工程经验，本文的总结纯粹是根据自己在网上所看文章的自己的理解，道听途说，请君自行分辨是非对错，阅读的文章会附于文末*

作为一个程序员，我讨厌黑盒，特别是当这个黑盒不那么省心，你无法在完全不了解黑盒的运作机制下完成工作的时候，它就变成了一个负担。譬如说上学期在做Java web的时候用到的Spring Boot在我眼中就是这样一个黑盒，至少在初学之后我只能战战兢兢地在他规定的范围内写配置，写代码。本文不探讨Java web、Spring Boot，只是略作吐槽。

几个月前，当我开始学习Vue并试图用Vue和Koa做出一套博客系统（就是cai-blog）的时候，我像以往一样搜了教程，vue-cli之后就开始往src文件夹下添代码。在不久之后，我放弃了cai-blog，返过头去重新学习HTML、CSS、JavaScript(ES5部分)，ES6包括HTTP等方面的知识。原因之一是我对vue文档里的

> 官方指南假设你已了解关于 HTML、CSS 和 JavaScript 的中级知识。

这句话有些心虚。另一个原因是我想知道vue-cli到底做了些什么。vue init webpack xxx这行命令做了什么，为什么要这样做。我想知道这些。

我第一次接触前端的时候应该是在2016年，在当时ES6标准已经推出，MVVM框架大行其道，Vue、React、Angular三足鼎立。各种各样的构建工具也早就如雨后春笋冒出，这里的雨可以说就是Nodejs。总之前端开发已经告别了刀耕火种的时代，告别了简单作为后端MVC框架中的V存在。

入坑前端开始，我被告知需要学习HTML、CSS、JavaScript这前端三剑客。但是当我分别单独学习完这三剑客之后。我面临了一个很尴尬的问题，我要怎么写出一个web app?是的，没错我已经知道了怎么在html中link入css，script入js文件，我知道怎么做一些简单的呆萌的静态网页。但是我要怎么做出一个稍微复杂的网站、web app?很明显，如果单纯用原生html、css、JavaScript去构建一个稍微大型的应用是几乎不可能的，开发效率太低了。那么，用jQuery?在html中script进一个jQuery库的js文件。jQuery库帮你解决了很多浏览器兼容问题、提供了一个便利的DOM选择器、封装了ajax请求库、提供了很多出色的动画。在MVVM框架出现之前，大部分的网站的构建方式应该就是类似以下过程产出的：

> 后端采用诸如Spring的MVC框架，在V这层引入一些后端模板引擎，比如velocity、thymeleaf。这类模板引擎的作用就是将后端处理后的数据render到模板中生成html。同时这份html当中link了css,script引入了js。前端工程师的工作大概就是用jQuery之类的库,写好模板引擎文件，引入写好的css,js，图片之类的静态资源，然后把写好的模板文件交给后端去部署。在网站前端的业务逻辑不太复杂，网站主要作为展示使用的时候,再加上jQuery带来的开发效率的提升，这种刀耕火种的方式还是较能满足业务需求的。

当我开始接触前端的时候，也就是2016年，使用ES5+jQuery的开发方式已经渐渐地"过时"了。事实上目前大多数还在运行的网站还是使用这样的开发方式构建的，所以远远谈不上过时。不过，MVVM是前端的未来，或者就说是前端的现在，基本上也是毫无争议的了。基本上从2015年或者更早，伴随着nodejs、ES6、MVVM框架、单页应用、webpack等等层出不穷前端的概念的流行。我们开发前端应用的方式已然发生了改变。这便是我今天的主题：前端工程化

## 前端工程化是什么

当提到前端工程化，不同人有不同的看法。说实话我觉得前端工程化是什么这个问题太过宽泛，实在不好回答。只能说在我的理解中它很大程度是为了提升开发效率，把一些本来需要人手动去完成的工作的繁琐又重复事情的交给机器去完成，解放程序员的生产力使其专注于业务逻辑。


### 模块化、组件化、资源管理
前端工程化要做的事情很多，包括但不限于模块化、组件化、资源管理。

JavaScript在ES5以前是没有模块化机制的。这也就是他没有像C/C++一样有#include。不像Java有package com.xx...我们谈前端工程化的时候，模块化应该是一个很重要的议题。因为在一个大型的前端工程中各个工程师的分工合作，以及模块化带来的分而治之的思想在大型工程中是十分实用的。

为了提升前端开发的效率，很多框架都提出了组件化开发的思想。比如我比较熟悉的vue。组件化开发就是vue很重要的一部分。这里我不做过多介绍，可以参考文末的文章。只想吐槽一句：前端程序员长期处于程序员鄙视链的底端，因为逢人都被说前端门槛低。作为一个略微学过安卓开发的人，实在不明白像安卓这样，从出生就是组件化开发的，到底入门的门槛比前端开发高在哪了。随便看本《第一行代码》，也能依葫芦画瓢搭积木似做出一个能用的app。作为一个初学前端的人，大部分只学过原生html、css、js，几乎什么像样的都做不出来。

因为前端代码无需编译，只要一个文本编辑器写完保存用浏览器打开就可以看到效果了。所以前端向来也不需要IDE，诸如目录结构、资源管理之类的脏累活也就只能手动来完成。同样对比安卓，Android Studio一开始当你create project之后就帮你把目录结构建好，什么图片应该放在哪个文件夹都帮你规划好。你只要负责往src里填写代码就行可。有没有发现.....这和vue-cli做的事好像，嗯，没错，至少我目前的观察发现。所谓前端工程化，不过就是如此，在Android或者Java Web。IDE帮你做好的许许多多的事情就是前端工程化所做的事情。

### 前端工程化的工具

从2014年以来，诞生了很多前端工程化的工具，Grunt、Gulp、Browserify等等。很不幸的事，在我还没有了解他们的时候，他们就已经死了。当我开始接触前端的时候，业界已经基本上都使用webpack来完成前端工程化，据说webpack诞生之初只是和Browserify一样是个bundler。这方面的知识我也不了解。现在我的心里差不多已经默认**只依靠webpack就能很好地完成大部分前端工程化要做的事**当然这句话可能之后就会被我自己证明是错误的，不过暂且这样认为吧。

在2017年末的时候，github上横空出世了一个项目[parcel](https://github.com/parcel-bundler/parcel)。迅速地吸引了大量的关注，它具有很多令人惊艳的特性。未来可能取代webpack，给前端带来一个真正省心的工具，让前端可以专注于业务逻辑。不过就目前而言(2018-02-19)，为了避免成为填坑侠，我们只能拭目以待。

## webpack

本文不讨论webpack怎么配置怎么使用，关注点在于我们为什么要多此一举使用webpack，直接编辑器写完保存，浏览器打开不好吗以及webpack到底在背后为我们做了什么?

首先，我们可以认为目前大多数浏览器只能直接运行html、css、JavaScript(ES5)。引用美团点评的技术文章中的一句话

> 鼓励将JavaScript、CSS、HTML视为前端领域的“汇编”。

我们可以当作浏览器只能运行html、css、JavaScript这样的“机器语言”。

显然直接用机器语言来开发程序的开发效率是极其低下的。同时html、css、JavaScript存在许多设计上的缺陷。所以我们需要一些“高级语言”来开发，类似jade、stylus、typescript就是这样的高级语言。高级语言写完代码之后，显然我们要把他们编译成html、css、JavaScript这样的机器语言才能供浏览器运行。

如果手动编写.stly文件编完之后再用stylus的编译器编译成css。那么对于.pug或者.ts甚至ES6。我们都得分别去用他们各自的编译器编译，然后还得在html中手动引入，当我们需要修改比如说.styl文件的时候我们需要修改源文件，修改完再重新编译成css。实在是特别麻烦、浪费时间、开发效率低下。所以webpack就用loader来解决这部分开发痛点。

1. 现代前端开发会使用像jade、pug来代替直接写html。写完jade或pug之后再编译成html，在webpack中完成这一过程我们需要loader。同理css存在没有变量，没有模块化设计等问题，
2. 现代前端开发会使用开发体验更好的stylus、sass等来代替直接编写css，同理之后需要编译的步骤，但是这并没有解决模块化问题，webpack的做法是把css文件也当成js module。利用js的模块化技术来模块化css。
3. 现代前端开发会使用ES6的一些特性，或者使用像typescript来代替直接编写原生JavaScript。由于一些浏览器还未支持原生ES6所以我们需要诸如Babel这样的工具，我们也可以很容易在webpack中使用babel
4. 类似Vue框架的单文件组件.vue也是无法直接在浏览器上运行的，所以Vue提供了vue-loader。我们可以在webpack中使用vue-loader来加载.vue
5. ......

除了将“高级语言”编译成“机器语言”的loader外，webpack其实还做了很多时来提高开发效率。譬如我们在发布静态资源的时候可能会对静态资源进行压缩，比如对js文件的话会使用uglify.js。但是如果每次写完js文件都得手动压缩，也是一件累人的事。所以我们可以在webpack的配置中添加ubglify的plugins。这样每次webpack命令编译也会顺带压缩js。除此之外还有HMR等等。也是用来提升开发效率的。可以知道的是webpack本身是一个功能强大，配置繁琐的工具，本文所介绍的肯定只是他的冰山一角。其次是提升开发效率是一个永无止境的话题，因为人类是懒惰的，能让机器完成的事就要尽量不让人类自己来做。

### node-dev-server

我们知道前端代码最终是放在服务器上的，在前端工程化中经常会强调前端要独立部署，独立运维。还有谈到前后端分离，前后端并行开发的时候。我们会谈到，在前端工程师的本地机器里完全搭建出一个后端的环境是不现实（就像在翻转课堂管理系统中我们在开发前端小程序的时候还在本地抛弃了一个spring app），所以既然前端想要独立部署，那么很明显我们得在本地搭建一个dev server。一般来说这个dev server就是一个静态资源服务器。我们使用webpack编译之后的html、css、js、图片等等就会放在这个静态资源服务器，然后我们再访问localhost:3000之类地去访问这个静态资源服务器上的文件。这时候可能还需要一些mock数据什么的。。

### 结语

前端工程化是一个宽泛的话题，怎样做前端工程化也没有一个放之四海而皆准的套路，对于不同的公司不同的团队，还是需要根据具体的项目不同来选择适合自己的套路。我始终觉得这个话题像我这样一个没有任何大型项目经验的学生来谈是很心虚，但是今日实在耗费很多时间在了解这个话题，仅作记录，满足了自己“想知道vue-cli”帮我干了什么事也好


#### 参考资料
[张云龙-前端工程——基础篇](https://github.com/fouber/blog/issues/10)
[美团点评-前端工程化开发方案app-proto](https://tech.meituan.com/tech-salon-13-app-proto.html)
[Webpack傻瓜式指南](https://github.com/vikingmute/webpack-for-fools)
[基于webpack搭建前端工程解决方案探索](http://www.infoq.com/cn/articles/frontend-engineering-webpack)