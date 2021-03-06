---
layout: post
title: "React Study Notes"
update: 2019-05-24
tags: [react, jsx, studynotes]
starMark: true
comments: true
date: 2019-03-31
---

> Study notes while learning React.

{% raw %}

## VS Code Shortcuts

imrc --

```jsx
import React, { Component } from "react";
```

cc --

```jsx
class  extends Component {
    state = {  }
    render() {
        return (  );
    }
}

export default ;
```

## Avoid Rendering Root Element

`<React.Fragment>`

JSX can only have one root element

All elements could be put inside a pair of <div> tag or `<React.Fragment>` tag.

if using <React.Fragment>, root element won't be incluede in html

## Get Random Pictures with fixed size

'https://picsum.photos/200'

Return a random picture of 200 by 200

## Add Class in JSX

Class--js = `ClassName`--jsx

```jsx
<span className={this.getBadgeClasses()}>{this.formatCount()}</span>
```

## Style

```jsx
style = {
    fontSize: 10,
    fontWeight: "bold",
};

<span style={this.style}> </span>;
```

<br>

```jsx
<span style={{ fontSize: 10, fontWeight: "bold" }}></span>
```

## Quickly create new method in render()

```jsx
let classes = "badge m-2 ";
classes += this.state.count === 0 ? "badge-warning" : "badge-primary";
```

Select and right click, then choose `"Refactor"`.

Do NOT write variable and do function inside `render()` mehod.

## Loop -- map

**no loop in jsx**

array.map(item => (//do something))

> each item needs a 'key' value for React

```jsx
tags: ["tag1", "tag2", "tag3"]

......
<ul>
  {this.state.tags.map(tag => (<li key={tag}>{tag}</li>))}
</ul>
```

## no if...else... in jsx

Alternatively, use `:` operator

```jsx
classes += this.state.count === 0 ? "badge-warning" : "badge-primary";
```

### trick in js

    true && false
    >false

    true && 'Hi' ==
    >"Hi"

    true && 'Hi' && 1
    >1

## binding event handlers

> This is used for when need 'this' to modify/update 'state'

Do not write function a directly. Using arrow functions instead. Casuse `this` keyword return `undefine` in a function, and what we need is returning an object that should write inside a class or object.

Arrow functions don't rebind `this` keyword. They inherieted.

```jsx
handleIncrement = () =>{
    console.log('Increment Clicked', this);
  }

//this is incorrect
handleIncrement(){
  .....
}
```

## passing event arguments

jsx don't call function in onClick(Upper case 'C') event, but using reference, which means no `()` when calling a function.

So if we need to pass arguments, arrow function comes.

```jsx
handleIncrement = (id) =>{
    console.log(id);
    this.setState({ count : this.state.count + 1});
    //tell React state has been changed. Merge or overwrite state
  }

render(
...
<button onClick={() => this.handleIncrement(id) } className="btn btn-secondary btn-sm">Increament</button>
...
)
```

## Zen Coding

Shortcut to get following code:

`table.talbe>thead>tr>th*4` then press `tab`

```html
<table className="talbe">
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th></th>
            <th></th>
        </tr>
    </thead>
</table>
;
```

## Passing data

`this.props`

Each react component has a property called `props` which basically is plain `JavsScript` object

## state vs props

\*`state` can only modified by its owner.

\*`props` is an object from parent's component and read only.

## Lifecycle Hooks

### Mounting Phase

1. `constructor` is only called once at first (before `render`), to initialize the properties in this class.

```jsx
    constructor(props){
        super(props);
        console.log("App - Constrctor", this.props);
        this.state =  this.props.__something___;
    }
```

2. `render` renders React elements from virtual DOM into brower DOM

3. `componentDidMount` is call after conponent is rendered into DOM, good place to do AJAX call to get data from server

```jsx
    componentDidMount(){
        //AJAX call
        this.state(_____)
    }
```

### Updating Phase

`componentDidUpdated` is called AFTER DOM updated, used to do something if needed following component changed

```jsx
    componentDidUpdated(prevProps, preState){
        if(prevPros.value !== this.props.value){
            //Ajax call and get new data from the server
        }
    }
```

### Unmounting Phase

`componentWillUnmount` is called BEFORE a component is removed from DOM, used to clean up set up timers or listener thoese staying in memory, otherwise they will be end up with memory leaks.

{% endraw %}
