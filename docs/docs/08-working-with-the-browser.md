---
id: working-with-the-browser
title: Working With the Browser
permalink: docs/working-with-the-browser.html
prev: forms.html
next: more-about-refs.html
---

React provides powerful abstractions that free you from touching the DOM directly in most cases, but sometimes you simply need to access the underlying API, perhaps to work with a third-party library or existing code.

React 提供了强大的抽象，使用户在大多数情况下不需要接触真实的DOM，但是某些情况下你却需要访问底层的API，例如和第三方库或已经存在的代码一同工作

## The Virtual DOM

React is very fast because it never talks to the DOM directly. React maintains a fast in-memory representation of the DOM. `render()` methods actually return a *description* of the DOM, and React can compare this description with the in-memory representation to compute the fastest way to update the browser.
React非常快，因为它不直接接触DOM。它在内存中保留了一份DOM的呈现形式。`render()`方法实际上返回的是DOM的描述，React可以比较这个描述和内存中的DOM形式（虚拟DOM）的不同之处，然后计算出最快捷的方式来更新浏览器（真实DOM）

Additionally, React implements a full synthetic event system such that all event objects are guaranteed to conform to the W3C spec despite browser quirks, and everything bubbles consistently and efficiently across browsers. You can even use some HTML5 events in older browsers that don't ordinarily support them!
另外，React 实现了完全的人造事件系统，使得事件对象和W3C标准相一致，尽管浏览器存在怪异模式，所有事件在不同浏览器中始终如一且高效地冒泡，你甚至可以在老的浏览器中使用一些HTML5事件，尽管这些老浏览器原生不支持这些事件。

Most of the time you should stay within React's "faked browser" world since it's more performant and easier to reason about. However, sometimes you simply need to access the underlying API, perhaps to work with a third-party library like a jQuery plugin. React provides escape hatches for you to use the underlying DOM API directly.

## Refs and findDOMNode()

To interact with the browser, you'll need a reference to a DOM node. You can attach a `ref` to any element, which allows you to reference the **backing instance** of the component.  This is useful if you need to invoke imperative functions on the component, or want to access the underlying DOM nodes.  To learn more about refs, including ways to use them effectively, see our [refs to components](/react/docs/more-about-refs.html) documentation.
为了和浏览器交互，你需要取得DOM节点的引用。你可以在任何元素上加上`ref`属性，这允许你引用组件背后生成的实例。当你需要调用组件的方法或访问底层的DOM节点时很有用。可以参考[refs to components](/react/docs/more-about-refs.html)

## Component Lifecycle

Components have three main parts of their lifecycle:

* **Mounting:** A component is being inserted into the DOM.
* **Updating:** A component is being re-rendered to determine if the DOM should be updated.
* **Unmounting:** A component is being removed from the DOM.
组件的3个生命周期：
* **Mounting** 组件将要挂载到DOM上时
* **Updating** 组件需要被重渲染，来确定DOM是否需要被更新
* **Unmounting** 组件被从DOM中移除

React provides lifecycle methods that you can specify to hook into this process. We provide **will** methods, which are called right before something happens, and **did** methods which are called right after something happens.
React提供了生命周期的钩子来供程序员调用。包含will的在事件发生之前调用，包含did的事件在事件发生之后调用
### Mounting

* `getInitialState(): object` is invoked before a component is mounted. Stateful components should implement this and return the initial state data.
* `componentWillMount()` is invoked immediately before mounting occurs.
* `componentDidMount()` is invoked immediately after mounting occurs. Initialization that requires DOM nodes should go here.

* `getInitialState(): object`在组件被挂载之前调用，有状态的组件应该实现这个方法并返回初始state
* `componentWillMount()` 在组件被挂载之前调用
* `componentDidMount()` 在挂载发生后调用。需要DOM节点的初始化需要在从这开始。
### Updating

* `componentWillReceiveProps(object nextProps)` is invoked when a mounted component receives new props. This method should be used to compare `this.props` and `nextProps` to perform state transitions using `this.setState()`.
* `shouldComponentUpdate(object nextProps, object nextState): boolean` is invoked when a component decides whether any changes warrant an update to the DOM. Implement this as an optimization to compare `this.props` with `nextProps` and `this.state` with `nextState` and return `false` if React should skip updating.
* `componentWillUpdate(object nextProps, object nextState)` is invoked immediately before updating occurs. You cannot call `this.setState()` here.
* `componentDidUpdate(object prevProps, object prevState)` is invoked immediately after updating occurs.

更新过程：
* `componentWillReceiveProps(object nextProps)` 在一个已挂载的组件接收新的props时候调用。

### Unmounting

* `componentWillUnmount()` is invoked immediately before a component is unmounted and destroyed. Cleanup should go here.

### Mounted Methods

_Mounted_ composite components also support the following method:

* `component.forceUpdate()` can be invoked on any mounted component when you know that some deeper aspect of the component's state has changed without using `this.setState()`.

## Browser Support

React supports most popular browsers, including Internet Explorer 9 and above.

(We don't support older browsers that don't support ES5 methods, but you may find that your apps do work in older browsers if polyfills such as [es5-shim and es5-sham](https://github.com/es-shims/es5-shim) are included in the page. You're on your own if you choose to take this path.)
React支持IE9及以上的现代浏览器。
