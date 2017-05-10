---
layout: post
title: React学习（三）-- 更多核心概念 
date: 2017-05-10
categories: blog
description: 前端学习
---

# React学习（三）-- 更多核心概念          

#### 别去改动props          
对这个组件的输入称为 props - "properties"的缩写。得益于 JSX 语法，它们通过参数传递。你必须知道在组件里，这些属性是不可改变的，也就是说 this.props 是只读的。          

如果你不知道要设置哪些 Props，那么现在最好不要设置它：          

```
  var component = <Component />;
  component.props.foo = x; // 不好
  component.props.bar = y; // 同样不好
```
这样是反模式，因为 React 不能帮你检查属性类型（propTypes）。这样即使你的 属性类型有错误也不能得到清晰的错误提示。          

Props 应该被认为是不可变的。在别处修改 props 对象可能会导致预料之外的结果，所以原则上这将是一个冻结的对象。          


#### 没有 JSX 的 React          
JSX完全是可选的；你无需在 React 中必须使用 JSX。你可以通过 React.createElement 来创建一个树。第一个参数是标签，第二个参数是一个属性对象，第三个是子节点。          

```
var child1 = React.createElement('li', null, 'First Text Content');
var child2 = React.createElement('li', null, 'Second Text Content');
var root = React.createElement('ul', { className: 'my-list' }, child1, child2);
ReactDOM.render(root, document.getElementById('example'));
```
方便起见，你可以创建基于自定义组件的速记工厂方法。          

```
var Factory = React.createFactory(ComponentClass);
var root = Factory({ custom: 'prop' });
ReactDOM.render(root, document.getElementById('example'));
```
React 已经为 HTML 标签提供内置工厂方法。          

```
var root = React.DOM.ul({ className: 'my-list' },
             React.DOM.li(null, 'Text Content')
           );
```

#### 深入 JSX          
JSX 是一个看起来很像 XML 的 JavaScript 语法扩展。          

由于 JSX 就是 JavaScript，一些标识符像 class 和 for 不建议作为 XML 属性名。作为替代，React DOM 使用 className 和 htmlFor 来做对应的属性。          

#### 命名组件（Namespaced Components）          
为了使其更简单和容易，命名组件令你使用包含其他组件作为属性的单一的组件。          

```
var Form = MyFormComponent;

var App = (
  <Form>
    <Form.Row>
      <Form.Label />
      <Form.Input />
    </Form.Row>
  </Form>
);
```
要做到这一点，你只需要把你的"子组件"创建为主组件的属性。          

```
var MyFormComponent = React.createClass({ ... });

MyFormComponent.Row = React.createClass({ ... });
MyFormComponent.Label = React.createClass({ ... });
MyFormComponent.Input = React.createClass({ ... });
```


## JavaScript 表达式          
要使用 JavaScript 表达式作为属性值，只需把这个表达式用一对大括号 ({}) 包起来，不要用引号 ("")。          

#### Boolean 属性:          
省略一个属性的值会导致JSX把它当做 true。要传值 false必须使用属性表达式。这常出现于使用HTML表单元素，含有属性如disabled, required, checked 和 readOnly。          

```
// 在JSX中，对于禁用按钮这二者是相同的。
<input type="button" disabled />;
<input type="button" disabled={true} />;
```

```
// 在JSX中，对于不禁用按钮这二者是相同的。
<input type="button" />;
<input type="button" disabled={false} />;
```

#### 子节点表达式 #          
同样地，JavaScript 表达式可用于描述子结点：          

```
// 输入 (JSX):
var content = <Container>{window.isLoggedIn ? <Nav /> : <Login />}</Container>;
```

#### 注释          
JSX 里添加注释很容易；它们只是 JS 表达式而已。你仅仅需要小心的是当你在一个标签的子节点块时，要用 {} 包围要注释的部分。          

```
var content = (
  <Nav>
    {/* child comment, 用 {} 包围 */}
    <Person
      /* 多
         行
         注释 */
      name={window.isLoggedIn ? window.name : ''} // 行尾注释
    />
  </Nav>
);
```

#### 自定义 HTML 属性          
如果往原生 HTML 元素里传入 HTML 规范里不存在的属性，React 不会显示它们。如果需要使用自定义属性，要加 data- 前缀。          

```
<div data-custom-attribute="foo" />
```
然而，在自定义元素中任意的属性都是被支持的 （那些在tag名里带有连接符或者 is="..." 属性的）          

```
<x-my-component custom-attribute="foo" />
```
以 aria- 开头的 网络无障碍 属性可以正常使用。          

```
<div aria-hidden={true} />
```

#### 性能提醒           
你或许会担心如果一个拥有者有大量子级时，对于数据变化做出响应非常耗费性能。值得庆幸的是执行 JavaScript 非常的快，而且 render() 方法一般比较简单，所以在大部分应用里这样做速度极快。此外，性能的瓶颈大多是因为 DOM 更新，而非 JS 执行，而且 React 会通过批量更新和变化检测来优化性能。          

但是，有时候需要做细粒度的性能控制。这种情况下，可以重写 shouldComponentUpdate() 方法返回 false 来让 React 跳过对子树的处理。参考 React reference docs 了解更多。          

注意：          
如果在数据变化时让 shouldComponentUpdate() 返回 false，React 就不能保证用户界面同步。当使用它的时候一定确保你清楚到底做了什么，并且只在遇到明显性能问题的时候才使用它。不要低估 JavaScript 的速度，DOM 操作通常才是慢的原因。          