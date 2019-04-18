### https是什么？
https, 全称Hyper Text Transfer Protocol Secure，相比http，多了一个secure，这一个secure是怎么来的呢？这是由TLS（SSL）提供的，这个又是什么呢？估计你也不想知道。大概就是一个叫openSSL的library提供的。https和http都属于application layer，基于TCP（以及UDP）协议，但是又完全不一样。TCP用的port是80， https用的是443（值得一提的是，google发明了一个新的协议，叫QUIC，并不基于TCP，用的port也是443， 同样是用来给https的。谷歌好牛逼啊。）总体来说，https和http类似，但是比http安全。

### https做得怎么样？

一般来说网络安全关心三个问题， CIA， （confidentiality, integrity, availability）。那https在这三方面做的怎么样呢？https保证了confidentiality（你浏览的页面的内容如果被人中途看见，将会是一团乱码。不会发生比如和你用同一个无线网的人收到一个你发的数据包，打开来一看，就是你的密码啊银行卡信息啊），intergrity(你浏览的页面就是你想浏览的，不会被黑客在中途修改，网站收到的数据包也是你最初发的那个，不会把你的数据给换掉，搞一个大新闻)，最后一个availability几乎没有提供（虽然我个人认为会增加基础DOS等的难度，但是这个不值一提），不过https还提供了另一个A， authentication(你连接的是你连接的网站，而不是什么人在中途伪造了一个网站给你，专业上叫Man In The Middle Attack)。那https具体保护了啥？简单来说，保护了你从连接到这个网站开始，到你关闭这个页面为止，你和这个网站之间收发的所有信息，就连url的一部分都被保护了。同时DNS querying这一步也被保护了，不会发生你输入www.google.com,实际上跑到了另一个网站去了。（这个其实也属于authentication，我这里不是很确定，最开始还写错了一次，应该来说，https保护了DNS Spoofing 和DNS Cache Poisoning等DNS攻击）那么有哪些没有被保护的？你是谁，你访问了什么网站（这个就是anonymity,想要上不好的网站但是不被人知道？可以用VPN或者TOR，当然可能要付出金钱或者速度变慢的代价啦。）

### https怎么做到的？

这个就很复杂了。有兴趣的朋友可以看一下这个“The First Few Milliseconds of an HTTPS Connection”。我来简单介绍一下里面的一些手段。比如你如何确信这个网站是一个好网站？好网站就会有一个“好网站证书”，也就是certification，这个证书是由CA（certificate authority）颁布的，每次链接，网站都先去找CA拿一份证书，然后把这个证书一起发给客户，来证明自己的清白。也许你会问，万一是一个坏网站自己伪造的证书呢？这就要牵扯到RSA的公钥，私钥加密。不过，google的https是他们自己公司的一个CA发的，感觉怪怪的。总之，你基本可以相信这是一个好网站（历史上也有CA被入侵之类的事件发生）。这就是authentication（应该也是保护DNS的一步）。当然你也会需要向网站证明一下你自己的身份，然后你们就要决定用什么方式加密。加密的方式有很多种，比如各种AES啦什么的。客户告诉网站，我的浏览器支持哪些加密方式，然后网站选择其中一种，于是你们之间的数据就被加密了。你问我怎么选择的？我告诉你是随机的。你问我是伪随机吗，我不知道，伪随机的话会不会有一种qd的感觉？总之，这就是confidentiality。那怎么保证你的数据不被修改呢？这就要说到hash，hash算法可以把一个长长的数据变短，一般情况下，不同的长数据变成的短数据，是不一样的。哪怕长数据里面只变化了一点点，短数据也会差别很大（专业术语叫avalanche effect）。传输数据的时候，把这个短数据一并传了，对方就可以知道整个数据包是否被修改。当然这需要双方都提前知道一些并没有被传输的秘密。常用的hash有md5和SHA256等，md5相对来说不安全，length extenstion attack和collision都很容易。总之，这样一来，你可以知道中途数据没有被修改。这就是integrity。

### https足够安全吗？

最后这个https足够安全吗？世界上没有绝对的安全，首先我提到过，https本身不保证availability，而且别人也能知道你在上这个网站。同时，https本身想保护的东西也不是那么靠谱。例如赫赫有名的heartbleed，2014年的时候席卷全球。数据显示，前100的网站（我也不晓得怎么排的），44个受到heartbleed威胁，其中就有雅虎，stackoverflow这样的网站。当然我觉得黑客是不会黑掉stackoverflow的，黑掉了以后自己写程序遇到bug都不知道怎么办了。直到今天，还有的网站没有修复这个bug，而一些已经修复的网站，因为没有及时更换private key等原因，自以为安全了，其实和没修复一个样。当然，还有各种各样的安全隐患。比如提到的RSA加密，在某些情况下可以用wiener attack破解。其他的例如入侵CA，或者直接入侵用户的电脑（例如用ssh开remote root shell等）都非常有可能。一定还有很多真正的“黑”科技，答主也不了解了。
