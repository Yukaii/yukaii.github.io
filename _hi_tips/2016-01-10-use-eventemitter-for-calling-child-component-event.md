---
layout: post
title: Use EventEmitter for calling child component event from parent in React
date: '2016-01-10T05:55:26+08:00'
tags:
- react native
- eventemitter
tumblr_url: http://hi-tips.tumblr.com/post/137014836571/use-eventemitter-for-calling-child-component-event
---

In parent component, writes

```js
// in import package parts
import EventEmitter from 'EventEmitter';

//in your Component class
componentWillMount = () => {
  this.eventEmitter = new EventEmitter();
}

handleAction = () => {
  this.eventEmitter.emit('SomeEvent', { someArg: 'argValue' });
}
```

Then pass on this.eventEmitter as props to child component.

```js
<ChildComponent event={this.eventEmitter} />
```


Finally add a listener for handling events in child component.

```js
import Subscribable from 'Subscribable';

var ChildComponent = React.createClass({
  mixins: [Subscribable.Mixin],
  componentDidMount() {
    this.addListenerOn(this.props.events, 'SomeEvent', this.myEventHandler);
  },
  //some other code goes here
});
```

Note that ES6 style classes do not support mixins, so you must write your child component class in legacy `createClass` style.

via [http://stackoverflow.com/questions/31177366/react-native-call-function-of-child-from-navigatorios](http://stackoverflow.com/questions/31177366/react-native-call-function-of-child-from-navigatorios)
