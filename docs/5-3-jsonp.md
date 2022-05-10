
#### jsonp反制说明

该页面用于介绍HFish蜜罐中Web类蜜罐默认开启的JSONP反制能力。

#### 什么是jsonp

jsonp(JSON with Padding) 是 json 的一种"使用模式"，可以让网页从别的域名（网站）那获取资料，即跨域读取数据。

由于同源策略，跨域之间无法传递数据，但是Web页面上调用js文件时则不受是否跨域的影响。（不仅如此，凡是拥有src这个属性的标签都拥有跨域的能力，比如<script>、<img>、<iframe>等)

于是可以判断，当前阶段如果想通过纯Web端（ActiveX控件、服务端代理、Web socket等方式不算）跨域访问数据就只有一种可能，那就是在远程服务器上设法把数据装进js格式的文件里，供客户端调用和进一步处理。

我们已经知道有一种叫做JSON的纯字符数据格式可以简洁的描述复杂数据，更妙的是JSON还被JS原生支持，所以在客户端几乎可以随心所欲的处理这种格式的数据。

Web客户端通过与调用脚本一模一样的方式，来调用跨域服务器上动态生成的js格式文件（一般以JSON为后缀），显而易见，服务器之所以要动态生成JSON文件，目的就在于把客户端需要的数据装入进去。

客户端在对JSON文件调用成功之后，也就获得了自己所需的数据，剩下的就是按照自己需求进行处理和展现了，这种获取远程数据的方式看起来非常像AJAX，但其实并不一样

为了便于客户端使用数据，逐渐形成了一种非正式传输协议，人们把它称作jsonp，该协议的一个要点就是允许用户传递一个callback参数给服务端，然后服务端返回数据时会将这个callback参数作为函数名来包裹住JSON数据，这样客户端就可以随意定制自己的函数来自动处理返回数据了。

这种方式极大的方便了前端页面开发，但也带来安全风险，如果正常网站开发者错误的暴露含有用户隐私（通常是用户名）的jsonp接口，蜜罐可以在虚假Web页面上构建而已js文件，诱使来访者浏览器访问正常网站jsonp接口从而得到来访者在正常网站上的真实身份。

因此，该技术通常用来溯源访问蜜罐的攻击者身份。

#### HFish蜜罐中的jsonp

HFish蜜罐中所有Web类蜜罐都默认开启jsonp反制代码，具体位于Web类蜜罐目录中的portrait.js代码中，

该文件是jsonp溯源功能的利用代码，攻击者在已登录其他社交平台的情况下，成功利用会让蜜罐获得部分社交平台的账号信息。

本代码因为利用了Chrome内核浏览器v80版本之前的漏洞，具有一定的时效性，随着攻击者更新自己的浏览器，利用代码可能失效，并有可能让攻击者在访问该页面时触发其杀毒软件报警。

您可以选择删除index.html中引用portrait.js的部分代码，或者自行优化portrait.js代码，补全更多反制方法。

HFish蜜罐社区非常期待用户贡献漏洞利用代码，扫码加入HFish蜜罐微信群：

![20210728203437](https://hfish.net/images/20210728203437.png)