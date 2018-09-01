---
layout: post
title: react项目实战总结
date: 2017-06-16
categories: blog
description: 前端学习
---

# react项目实战总结          
最近进行了公司产品2.0的两周开发，一期也算是告一段落了，下周就开始进行二期的开发了，今天正好空出来一下午总结一下这两周做项目中值得注意的一些点。          

### 首先，编写路由          
路由都是在全局的一个routes.js的文件中统一写的，写了对应的url和模板文件。          

### 暂未用到redux          
我负责的部分主要是内部数据较多，但并未有全局互通的数据，因此暂未用到redux，也算是降低了难度。在决定要使用redux之前，确实也要非常小心，仔细想一想到底需不需要redux，若不需要就不要用避免增加代码难度。          

### tabs，menus          
在我的页面中，会有几个tab或者menu，他们的样式虽然不同，但思想都是通过切换改变url进行不同的模板切换，子模板内容都是通过this.props.children呈现的，这也是react-router的语法。          

### react-router和react-router-redux          
虽然在我的部分没有用到，但还是要了解下：[http://www.8dou5che.com/2017/01/22/react-router-redux/](http://www.8dou5che.com/2017/01/22/react-router-redux/)。          
一句话解释说就是我们在项目中使用redux和react-router时，这两个库不是很协调，无法满足时间旅行的要求，而react-router-redux就是将他们保持同步的库。          

### 什么都是模块          
在es6引进了模块系统之后，模块化的思想在项目中被运用得淋漓极致，什么都成了模块：一个接口函数、一张图片、一段样式代码：          

``` javascript
// 引用
import { headBarInfo } from './api';
import headBarLogoDefault from './images/head-bar-logo-default.png';
// headBarLogoDefault就是打包后的相对路径
import styles from './styles.css';

// 使用
headBarInfo().then(() => {});
<img src={headBarLogoDefault} className={styles.someClassName} />
```

### 对数据进行处理          
在我负责的页面中，数据多得难以形容，每个文件光是把各个数据在state里写一遍再在拿到数据后setState一遍就已经一辆百行了，而每个数据都会出现很多特殊情况需要处理：undefined、空字符串、空数组、数组只有一个空值、空对象，这些种种数据都要进行处理。          
刚开始写代码时，我是遇到一个数据，若有特殊情况就处理一次，但写到后来，发现需要处理的代码越来越多，随着后期一些需求的变动，甚至又需要重新进行处理，这个工作量是非常大的。          
到了后来，我对绝大部分数据进行了统一的处理，即通过API拿到数据之后就将其处理成合理的格式，再将合理格式的数据setState，这样后面无论多少次使用这个数据，都不需要在数据时再进行判断。          
没有一开始就统一处理数据也让我这次多花了很多时间，以后一定要注意这个问题，吸取教训。还有就是说，一定要在最开始就和后台和产品讨论好可能出现的数据类型，避免返工。          

### 保持render函数的纯净          
有些数据，比如小于30天时，就要处理成 ”<一个月“，在刚开始写时，我是写在render函数中的，以致于写到后来，render函数还有很多处理数据的逻辑，这是有违react的思想的，react希望render函数能够尽量的纯净，只负责渲染的逻辑，而一些其他的比如处理数据的逻辑大可以写到外面写成一个函数。          
比如我需要对一个month字段进行处理，就在render函数外写一个getMonth函数就好，需要用到时再通过this.getMonth()调用。          

### 保持函数类型较少          
在各个页面中，我这次只使用了四种类型的函数：1、componentDidMounted（），用来处理渲染之后的逻辑。2、getSomething（），用来处理数据并返回，统一用get开头。3、handleSomeClickAndSoOn（），用来处理一些点击、勾选事件等，统一用handle开头。4、renderSomething（），用来渲染某一部分，统一用render开头，在其中几乎没有处理数据的逻辑。          

### 写好注释          
经常写快了就会忘记写注释，虽然自己也看的懂，但以后要是多人开发同一页面时，不及时写注释会增加协作的难度，这个一定要强迫自己以后及时写好注释。          

### DRY          
当一些代码是实现相同的功能或者觉得某两段代码较为相似时，多想想是否能够提取出通用的部分，提取为一个函数或组件，这样可以少写相同的代码，也使代码可复用性更高。          

### JSX语法中，尽量不要在样式中添加判断          
比如下面这段代码：          

``` javascript
<div style={this.state.show ? { display: 'block' } : { display: 'none' }}>
 lala
</div>
```
这么写的问题中，在style中还加进了逻辑，并且这整段代码都会执行，而改成下面这样写之后：          

```
{this.state.show ? ( <div>lala</div> ) : null}
```
不仅写法更简单，而且当this.state.show为false时，括号里的内容都不会执行，特别是当其中有很多要渲染的内容时，这样写就避免了执行一大段不必要代码的情况。          

### target=“blank”          
以前写< a >时，只知道加一个target=“blank”，这个链接就会在一个新窗口中打开，但是，如果有两个链接都是设置的这个属性，点击第一个链接时，会打开一个新的标签页，但再点击第二个链接时，会在这个新标签页中覆盖第一个打开的链接。这是因为target属性是说将这个链接链接到这个target，如果没有这个target，就打开一个新标签页，而要是target的值设得相同时，以后每次打开都是到这个标签页了。因此，这种情况最好还是设成target=“page1”，target=“page2”更好。          

### 通过ref拿到真实的DOM节点          
尽量少用，真的，操作DOM本来就不是react的思想。我这也是第一次用到，确实没其他更好的办法了。需求是这样的：我需要在页面加载完后读取到某个div的内容的高度，若是多于三排，就显示展开和收起按钮，不然就不显示。因此我需要拿到真实数据在页面中的高度，这个必须要用到真实的DOM节点，因此在这种情况下使用了一次ref。值得一提的是，当height没有设置，根据内容自动变化时，通过someNode.style.height是拿不到值的，只能通过clientHeight、scrollHeight、offsetHeight属性来拿，当然，它们三个属性也是有差别的。          

### 数据驱动          
始终记得，react的思想是数据驱动的。          

### css的优先级          
尽量别用 !important，这样会写死的，以后可能会出现别人的代码在不知情的情况下被改动或者无法更改的情况。通过特殊性的计算法则，增加类名、id、从属关系等来让样式可以显示出来。          

### 函数的提取          
注意放到同一层级下的函数的功能和从属要类似，避免出现拆分得太细的情况。比如有一个a函数和一个b函数是同级的，而a函数里有一部分内容也可以拆成一个c函数，但要是把a、b、c三个函数都写到同一级下又感觉不太妥当，所以还是要注意拆分函数时的必要性。          

![赞赏码.jpeg](https://upload-images.jianshu.io/upload_images/3001083-f65814d1f594b39c.jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)     
