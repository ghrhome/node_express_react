# React and ES6 - Part 3, Binding to methods of React class \(ES7 included\)

[http://egorsmirnov.me/2015/08/16/react-and-es6-part3.html](http://egorsmirnov.me/2015/08/16/react-and-es6-part3.html)

This is the third post of series in which we are going to explore the usage of React with ECMAScript6 and ECMAScript7.

You could find links to all parts of series below:

* [React and ES6 - Part 1, Introduction into ES6 and React](http://egorsmirnov.me/2015/05/22/react-and-es6-part1.html)
* [React and ES6 - Part 2, React Classes and ES7 Property Initializers](http://egorsmirnov.me/2015/06/14/react-and-es6-part2.html)
* **React and ES6 - Part 3, Binding to methods of React class \(ES7 included\)**
* [React and ES6 - Part 4, React Mixins when using ES6 and React](http://egorsmirnov.me/2015/09/30/react-and-es6-part4.html)
* [React and ES6 - Part 5, React and ES6 Workflow with JSPM](http://egorsmirnov.me/2015/10/11/react-and-es6-part5.html)
* [React and ES6 - Part 6, React and ES6 Workflow with Webpack](http://egorsmirnov.me/2016/04/11/react-and-es6-part6.html)

> The code corresponding to this article is available at[GitHub](https://github.com/egor-smirnov/egorsmirnov.me-examples/tree/master/react-and-es6-part-3).

> _Update from 18.06.2016:_Updated the code and text to use React 15 and Babel 6.

If you look at paragraph “CartItem render method” of the[previous article in the series](http://egorsmirnov.me/2015/06/14/react-and-es6-part2.html#cartitem-render-method)you might be confused by the usage of`{this.increaseQty.bind(this)}`.

If we try the same example with just`{this.increaseQty}`we’ll see`Uncaught TypeError: Cannot read property 'setState' of undefined`in browser console:

![](http://egorsmirnov.me/images/posts/2015-08-16/console.png "Browser console with error")

This is because when we call a function in that way binding to`this`is not a class itself, it’s`undefined`. It’s default JavaScript behavior and is quite expected. In opposite to this, in case you use`React.createClass()`all the methods are autobinded to the instance of an object. Which might be counter-intuitive for some developers.

No autobinding was the decision of React team when they implemented support of ES6 classes for React components. You could find out more about reasons for doing so in[this blog post](http://facebook.github.io/react/blog/2015/01/27/react-v0.13.0-beta-1.html#autobinding).

Let’s now see various ways of how to call class methods from your JSX in case you use ES6 classes. All the different methods we consider here are available at[this GitHub repository](https://github.com/egor-smirnov/egorsmirnov.me-examples/tree/master/react-and-es6-part-3).



## Method 1. Using of Function.prototype.bind\(\). {#method-1-using-of-functionprototypebind}

We’ve already seen this:

```
export default class CartItem extends React.Component {
    render() {
        <button onClick={this.increaseQty.bind(this)} className="button success">+</button>
    }
}
```

As any method of ES6 class is plain JavaScript function it inherits`bind()`from Function prototype. So now when we call`increaseQty()`inside JSX,`this`will point to our class instance. You could read more about Function.prototype.bind\(\) in this[MDN article](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

## Method 2. Using function defined in constructor. {#method-2-using-function-defined-in-constructor}

This method is a mix of the previous one with usage of class constructor function:

```
export default class CartItem extends React.Component {
    
    constructor(props) {
        super(props);
        this.increaseQty = this.increaseQty.bind(this);
    }

    render() {
        <button onClick={this.increaseQty} className="button success">+</button>
    }
}


```

You don’t have to use`bind()`inside your JSX anymore, but this comes with the cost of increasing the size of constructor code.

## Method 3. Using fat arrow function and constructor. {#method-3-using-fat-arrow-function-and-constructor}

[ES6 fat arrow functions](https://babeljs.io/docs/learn-es2015/#arrows)preserve`this`context when they are called. We could use this feature and redefine`increaseQty()`inside constructor in the following way:

```
export default class CartItem extends React.Component {
    
    constructor(props) {
        super(props);
        this._increaseQty = () => this.increaseQty();
    }

    render() {
        <button onClick={_this.increaseQty} className="button success">+</button>
    }
}
```

## Method 4. Using fat arrow function and ES2015+ class properties. {#method-4-using-fat-arrow-function-and-es2015-class-properties}

Additionally, you could use fat arrow function in combination with experimental ES2015+ class properties syntax:

```
export default class CartItem extends React.Component {
      
    increaseQty = () => this.increaseQty();

    render() {
        <button onClick={this.increaseQty} className="button success">+</button>
    }
}
```

So, instead of defining our class method in a constructor as in method number 3, we use property initializer.

**Warning:**Class properties are not yet part of current JavaScript standard. But your are free to use them in Babel using corresponding experimental flag \(stage 0 in our case\). Your could read more about how to do this in[Babel documentation](https://babeljs.io/docs/usage/experimental/).

We’ve already switched to stage 0 in[React and ES6 - Part 2, React Classes and ES7 Property Initializers](http://egorsmirnov.me/2015/06/14/react-and-es6-part2.html#bringing-es7-into-the-project), so this is not an issue for our example.

## Method 5. Using ES2015+ function bind syntax. {#method-5-using-es2015-function-bind-syntax}

Quite recently Babel introduced syntactic sugar for`Function.prototype.bind()`with the usage of`::`. I won’t go into details of how it works here. Other guys already have done the pretty good explanation. You could refer to[this official Babel blog post](http://babeljs.io/blog/2015/05/14/function-bind/)for more details.

Below is code with usage of ES2015+ function bind syntax:

```
export default class CartItem extends React.Component {
    
    constructor(props) {
        super(props);
        this.increaseQty = ::this.increaseQty;
        // line above is an equivalent to this.increaseQty = this.increaseQty.bind(this);
    }

    render() {
        <button onClick={this.increaseQty} className="button success">+</button>
    }
}
```

Again, this feature is highly experimental. Use it at your own risk.

## Method 6. Using ES2015+ Function Bind Syntax in-place. {#method-6-using-es2015-function-bind-syntax-in-place}

You also have a possibility to use ES2015+ function bind syntax directly in your JSX without touching constructor. It will look like:

```
export default class CartItem extends React.Component {
    render() {
        <button onClick={::this.increaseQty} className="button success">+</button>
    }
}
```

Very concise, the only drawback is that this function will be re-created on each subsequent render of the component. This is not optimal. What is more important that will cause problems if you use something like PureRenderMixin \(or its equivalent for ES2015 classes\).

## Conclusion {#conclusion}

In this article, we’ve discovered various possibilities of binding class methods of React components. I’ve prepared test project based on code from part 2 of this series. It’s available[here](https://github.com/egor-smirnov/egorsmirnov.me-examples/tree/master/react-and-es6-part-3).

Next time we’ll see what is the state of React mixins for ES2015 classes.

## Further Reading {#further-reading}

* [About autobinding in official React blog](http://facebook.github.io/react/blog/2015/01/27/react-v0.13.0-beta-1.html#autobinding)
* [Autobinding, React and ES6 Classes](http://www.ian-thomas.net/autobinding-react-and-es6-classes/)
* [Function Bind Syntax in official Babel blog](http://babeljs.io/blog/2015/05/14/function-bind)
* [Function.prototype.bind\(\)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
* [Experimental ES7 Class Properties](https://gist.github.com/jeffmo/054df782c05639da2adb)
* [Experimental ES7 Function Bind Syntax](https://github.com/zenparsing/es-function-bind)













