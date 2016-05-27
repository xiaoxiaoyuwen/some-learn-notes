# Visual Information Theory

Posted on October 14,2015

[原文链接](http://colah.github.io/posts/2015-09-Visual-Information/)
  
我喜欢用一种新的方式理解世界的感觉。尤其喜欢当一些模糊不清的想法正式的变成一种明确的概念的时候。信息论就是典型的例子。

信息论给了我们一种描述很多事物的精确语言。有多大的不确定性？知道了问题A的答案能够对问题B有多大的帮助？一组信念(one set of beliefs)与另一组有多大的相似度？在我还是个小孩的时候就有了这些非正式的想法(idea)，但是信息论使他们变成精确的、强大的ideas。这些ideas有广泛的应用，数据压缩、量子物理、机器学习等等等。

不幸的是信息论看起来是很恐怖(的理论)。我不认为它就应该是这样恐怖的。事实上，很多ideas完全可以可视化的解释。

## 概率分布可视化
在讲信息论之前，我们先简单的想象怎么可视化概率分布。后面需要用到它，现在处理起来也很方便。这种可视化概率分布的技巧本身也是非常有用的。

我在加利福利亚。有时会下雨，但是大部分时间都是晴天！假设75%的时间是晴天。我们可以简单的用图来表示：

<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/prob-1D-rain.png" width="50%" />
<p/>

大多数情况下我会穿T恤，有时也会穿外套。假设穿外套的概率是38%。则简单的图形表示为：
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/prob-1D-coat.png" width="40%" />
<p/>
怎么同时可视化天气和穿着的概率呢？如果这两件事情不相互影响--即相互独立的事件，那就很简单了。例如，我**今天**是穿T恤还是穿外套并不受**下周**天气的影响。我们可以使用两个坐标轴分别表示两件事情：
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/prob-2D-independent-rain.png" width="80%" />
<p/>
注意上图中横向和纵向的线都是笔直的。***这就是独立事件看起来的样子***！我穿外套的概率并不随下周要下雨的影响。换句话说，我穿外套并且下周下雨的概率等于穿外套的概率乘以下周下雨的概率。他们不相互影响。  
当变量相互影响时，会有一些概率分配到特殊的变量对上，相应的一些变量对会损失些许概率。比如，我穿外套并且下雨这件事就会有额外的概率，因为这两个变脸相互影响，使得彼此有更大的可能发生。相对而言我在雨天穿外套的概率要高于其他天气下穿外套的概率。  
可视化之，就像一些方形会膨胀而一些方形会被压缩。
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/prob-2D-dependant-rain-squish.png" width="80%" />
<p/>
尽管这种图看起来比较酷，但是不利于理解发生了什么。  
让我们将精力放在一个变量上，如天气。我们知道晴天和雨天的概率。我们可与观察下两种情况下的**条件概率**。在晴天的条件下我有多大的可能穿外套？在雨天的情况下又有多大的可能？
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/prob-2D-factored-rain-arrow.png" width="80%" />
<p/>
上图中下雨的概率是25%。如果下雨，有75%的可能我会穿外套，所以下雨的条件下穿外套的概率就是25%乘以75%，大概19%的概率。即下雨的概率乘以下雨时我穿外套的条件概率：
$$p(rain,coat)=p(rain)·p(coat|rain)$$
这是概率论里的一个基本的等式：  
$$p(x,y)=p(x)·p(y|x)$$
上面是一个分布的分解：分成两个部分的乘积。首先，只关心一个变量，如天气，一个确定的概率值。然后，观察另一个变量，如我的穿着，在前一个变量的条件下的一个概率值。  

首先观察哪个变量是随机的。完全可以考虑我的穿着，然后在穿着的条件下考虑天气。可能有一点绕，因为通常的因果关系是天气影响穿着而不是反过来的...but it still works（就是这么任性）  

再来过一遍这个例子。随机选择一天，38%的概率我会穿外套。如果现在知道我穿了外套，问：下雨的概率是多大？明显，相较于晴天，下雨的情况下我更有可能穿外套，但是加利福利亚很少下雨，计算出来其概率为50%（我穿外套的条件下下雨了$$$p(rain|coat)=\frac{p(rain,coat)}{p(coat)}$$$）。那么现在下雨了并且我穿外套了这件事的概率等于我穿外套的概率乘以穿外套的条件下下雨的概率，约等于19%。
$$p(rain,coat)=p(coat)·p(rain|coat)$$
这带来了第二种概率分布的可视化方式：
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/prob-2D-factored1-clothing-B.png" width="80%" />
<p/>
注意这里的标签与前面有点不同：T恤和外套现在是**边缘概率**：不管什么天气的情况下我穿外套和T恤的概率($$$p(coat)=p(coat|rain)+p(coat|sunny)$$$)。  

*译者注：上图中左边标记raining的概率应该为25%而不是8%，sunny的概率应该是75%*

## Aside: Simpson's Paradox 辛普生悖论
这些可视化概率分布的技巧是否真的有用？我想答案是肯定的！在用他们可视化信息论之前是有一点用的，下面将先使用它们解释 Simpson's paradox。Simpson's paradox 是一种非常不直观的统计现象。很难凭直觉去理解它。Michael Nielsen谢了一片很好的文章从不同的角度去解释它（[Reinventing Explanation](http://michaelnielsen.org/reinventing_explanation/)）。下面将使用前面介绍的可视化技巧来解释它。  

假设测试了两种治疗肾结石的方法。一半的病人使用A治疗方案，另一半使用方案B。使用方案B的病人有更大的概率被治好（总的统计结果）。
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/simpson-margin.png" width="80%" />
<p/>
但是，轻度的肾结石患者使用方案A得到治愈的概率更大。重度的肾结石患者使用A方案治愈的概率也更大！（条件统计结果）。这怎么可能呢？  
问题在于研究中的患者不够随机。接受A方案的患者患有重度肾结石的比较多，而接受B方案的患者语伴都是患有轻度肾结石的。  
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/simpson-participants.png" width="80%" />
<p/>
一般来说，患有轻度肾结石的患者治愈的可能性本来就比较大。结合前面两个直方图来理解。下面的三围图：
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/simpson-separated-note.png" width="80%" />
<p/>
可以看到，重度结石和轻度结石，方案A的治愈概率都是大于方案B的。方案B只是在总体上得治愈概率大于方案A。（好诡异哈）  

译者注：假设方案A中重度患者 40人，治愈30人，治愈率75%，轻度患者10人，治愈9人，治愈率，90%，总的治愈率78%。方案B中重度患者10人，治愈6人，治愈率60%，轻度患者40人，治愈34人，治愈率85%，总体治愈率80%。

## codes
既然可以可视化概率，我们可以深入信息论了。
假设有我有个朋友Bob，它很喜欢动物，总是不停的说动物的事情。假设他只说四个词：dog，cat，fish 和 bird。
假设Bob搬到澳大利亚去了，并且他只想通过而进制的方式通信。我从Bob得到的消息都是像下面这样的：
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/message.png" width="80%" />
<p/>
为了交流，我和Bob必须简历一套编码，将单词映射成二进制的序列。
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/code-2bit.png" width="80%" />
<p/>
为了发送信息，Bob需要将单词替换成对应的编码，然后将他们连起来形成编码串。
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/encode-2bit.png" width="80%" />
<p/>

## Variable-Length Codes
不幸的是，假设通信服务在澳大利亚很昂贵。$5刀每个bit。可是Bob又话多。为了防止我破产，我们需要调研有没有压缩我们通信长度的方式。  
事实上，Bob不是等概率的说上面四个词。Bob很喜欢狗。他总是谈论狗，偶尔会提及其他动物--特别是猫（他的狗喜欢追猫）--大部分情况下他都是在谈论狗。下面是他提及各个动物的概率图：
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/DogWordFreq.png" width="80%" />
<p/>
似乎很有前途。我们旧的编码方案都是2bit的，不管他们的概率如何都是2bit的。  
可视化之。下面的图中，使用纵坐标表示没个词的概率$$$p(x)$$$,横坐标表示编码的长度$$$L(x)$$$，注意面积表示编码的平均长度。  
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/OldCode.png" width="80%" />
<p/>

也许我们可以聪明的使用变长编码，将常用的词变成较短的编码。问题在于词之间的竞争--一些词变短必然导致另一些词变长。为了最小化信息长度，理想的是所有的词都变短，但是更希望常出现的词变短。所以常用的词编码变短，不常用的词编码变长。
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/code.png" width="80%" />
<p/>
再来看看编码长度的可视化。注意现在最经常出现的编码已经变短了，尽管不经常出现的编码变长了。可以看到图中总的面积变小了。相当于编码长度更短了。编码平均长度1.75bit！
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/NewCode.png" width="80%" />
<p/>
(你可能会问：为什么单独的1没有作为一个编码？因为这样在连续的编码串中会导致歧义)  
可以证明这是最好的编码方式。不会有别的编码方式能够获得比1.75bit更小的平均编码长度。  

一个最基本的限制。这种分布下，想要通信，平均至少需要1.75bit。不管你用多聪明的编码都不可能使这个编码长度更短了。这种基本的限制叫**熵**--后面详细讨论。

<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/EntropOptimalLengthExample.png" width="80%" />
<p/>

想理解这种限制，最难的是理解在缩短某些词的编码和增加另一些词的编码之间寻找平衡。一旦理解了，就能理解最好的编码可能是什么样的。  

## The Space of Codewords 编码空间
0和1的编码长度为1。其他的00、01、10、11编码长度为2。每增加一位编码，编码空间(编码个数)翻倍。
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/CodeSpace.png" width="80%" />
<p/>
我们喜欢可变长编码，即一些编码的长度大于其他的编码。简单的情况可能有8个编码，编码长度为3。复杂一点，2个编码长度为2的编码，4个编码长度为3的编码。那么是什么决定了我们能有多少个不同长度的编码呢？

前面说到，Bob使用编码替换单词并将编码连接起来组成编码串。
<p align="center">
<img src="http://colah.github.io/posts/2015-09-Visual-Information/img/encode.png" width="80%" />
<p/>
有个问题需要解决，碰到这种编码串怎样切分它才能还原出原来的codewords呢。codewords长度固定时很好解决。编码长度可变时就要小心处理了。  

我们希望编码能够唯一的被解码。不能让编码后的编码串存在歧义的问题。如果有一些特许的编码结束标记，问题就好解决了。但是没有，我们只是发送0和1。我们必须要能精确的切分编码串。  





















