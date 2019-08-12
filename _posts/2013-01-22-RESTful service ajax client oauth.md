前段时间，痴迷于RESTful风格，希望能在项目中事件一下，结果已失败告终。

不过从中还是得到了不少经验，拿出来分享一下。

首先RESTful的优点或者说特点是什么：

1，定义简单明了，对于同一个资源的操作，只需要一条URL就可以了，例如：http://example.com/cars

这样便可以对资源进行CRUD操作了。

2，RESTful属于一种WebService,是按照HTTP定义的WebService，也就是说按照普通的Web处理方式便可以实现WebService，一个网站很轻松的就可以转型到RESTful，也就是转换成WebService。进而把你的服务拓展到Web Broswer之外的客户端上。

那RESTful和Oauth有什么关系呢？

RESTful由于具有HTTP和WebService双重特点，所以他能利用HTTP的优点，但是由于WebService性质，他又要回避HTTP中的一些特性。这里最显著的冲突就是HTTP Session，众所周知，Session是利用cookie来保持对应的，而在非Web broswer之外的客户端上，可能不具有Cookie特性。因此HTTP的Session协议再次就无法作用。

而Oauth第三方授权协议，由于也无法使用Cookie，所以把Cookie转变成accessToken，这样就由客户端自己来实现持有accessToken的方法。

当然，如果RESTful服务器上全部是公开数据，无需授权便可以访问，那么就用不到Oauth协议。Oauth协议只是为了操控和用户相关的资源。

Ajax Client中遇到的问题？

有了上述基本知识之后，我便打算采用纯Ajax的方式做一个客户端，载体虽然还是浏览器，但是和服务器的交互全部交给了XMLRequest对象。这个想法确实很诱人，尤其是在V8引擎大行其道的年代。

但是问题随之出现，如果我从一个页面跳转到另外一个页面，或者刷新页面。accessToken便会丢失，因为页面的重新载入意味着JS对象的重建。不过这个问题好解决，我可以利用Cookie把accessToken存储起来，也可以利用最新的HTML5 localstorage来存储。

这就没有问题了？No，the world full of jokes.当XMLRequest对象向服务器发送请求的时候，它永远都会返回错误，因为你涉及到了跨域请求。这个问题，是一个综合性质的问题，首先你的WebService要介绍来自其他域的请求：
Access-Control-Allow-Origin:http:// example.com,
然后你的Web broswer也有具有跨域请求的能力，很荣幸，IE6再次上榜。如果这些都不能实现的话，你可以通过JSONP来获得数据。但那仅仅是获得数据，而无法发送数据。难道就没有解决办法了吗？

等等，没有不透风的墙。iframe要发挥他无限强大的能力了。通过内建一个iframe,并且访问到WebService所在的网页。这样，你的数据便可以通过 Client(Web Broswer)->proxy(iframe)->Server(RESTful) 这条路径和服务器进行通讯了。别的应该不用我说了，Weibo也是采用的这个方式。

Have a nice day!