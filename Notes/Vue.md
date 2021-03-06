# Vue JS

## Section 1: Introduction

* Vue is a Javascript framework which allows you to build anything from small widgets that can be inserted in larger projects to medium or fullsize enterprise projects.
* Vue is quite small and lean in terms of file size compared to other Javascript frameworks.
* Vue is extendable, it can increase in size and functionality depending on what the project requires.
* The Vue project exports a Vue object. This is Vue object can be imported and is the core of your vue application. By importing this object you create a new Vue instance. These istances have one major job: control a piece of code which they render to the screen.

To call a new Vue instance you use the following code. The `el` property is reserved and takes a `string` as a value. With this string we specify which part of our HTML should be under control of this Vue instance. This means that you can change this code through the Vue instance. The `data` property specifies what data you want to use in this Vue instance.
```
new Vue({
  el: '#app',
  data: {
    title: 'Hello World'
  }
})
```
```
<div id="app">
  <p>{{ title }}</p>
</div>
```

* A `directive` is command/instruction that Vue will recognize in order to perform a certain task. eg: `v-on`
* `v-on:input="changeTitle"` is a directive that tells Vue to listen to an input event. When the event is triggered the changeTitle event will be fired.

Stating a `method` in Vue object allows you to call that method in the HTML. All properties you need can be accessed using the `this` keyword. Vue automatically passes the event object to said method.
```
new Vue({
  el: '#app',
  data: {
    title: 'Hello World'
  },
  Methods: {
    changeTitle: function(event){
      this.title = event.target.value;
    }
  }
})
```
```
<div id="app">
  <input type="text" v-on:input="changeTitle">
  <p>{{ title }}</p>
</div>
```

## Section 2: Using VueJS to interact with the DOM

### The connection between a Vue instance and HTML code

* By creating/instantiating a specific Vue instance we create a connection between the Vue object and the specific HTML code.
* Vue takes the HTML code and creates a template based on that code. Vue does not add runtime or directly use the HTML code.
* Vue used the template based on the HTML code to do it's actions and then renders this to the actual DOM. This allows for templating and string interpolation `{{ title }}`.
* There is a layer between the rendered HTML code and the HTML we write. This layer is the VueJS instance which creates renders things into the HTML. `Written HTML code -> VueJS instance -> Final Rendered HTML code`.

### How the VueJS Template Syntax and Instance work together

* Data stored in the `data` property in the Vue instance can be outputted in the HTML using double curly braces `{{ title }}`.
* Methods stored in the `methods` property can be called in the HTML using double curly braces `{{ myFunction() }}` .
* Whatever you output in the curly braces has to be able to be converted into a string.

### Accessing Data in the Vue Instance

* To access properties in the Vue instance, you need to use the `this` keyword `this.title`.
* This works because behind the scenes Vue creates an easy access for us to these properties.
* In the HTML template you don't need to use `this`, in the Vue Object you do.

### Binding to Attributes

* In VueJS you can't use curcly braces `{{ link }}` in any HTML element attributes. You can only use them in the place where you would normally use text.
* For binding to attributes you can use the `v-bind` directive. This directive tells Vue to bind the attribute to a Vue property instead of reading the attribute normally `<a v-bind:href="link">Google</a>`. Using this you don't need curly braces because you're already reading the VueJS properties due to `v-bind`.

### Understanding and using Directives

* A Directive is an instruction you place in your code. Vue ships with a number of built in Directives. You can also write your own directives.
* For example, `v-bind` tells Vue to bind something to the data of the Vue instance (which included the functions/methods).
* Generally you pass an argument to directives using a colon `v-bind:href="link"` following with the attribute you want to bind. Then between the quotation marks you pass the name of the property/method that you want to pass from the VueJS instance.

### Disable re-rendering with v-once

* Usually when you change a property, for example `title`, all instances of that property will change too.
* You can stop this from happening using `v-once`. By adding this to an element all the content inside of this element will only be rendered once. It will not not be updated if anything changes.

```
<h1 v-once>{{ title }}</h1>
```

### Outputting raw HTML

* You can write HTML code in Vue properties `finishedLink: '<a href="http://google.com>Google</a>'`.
* By default VueJS escaped HTML, which means it doesn't render HTML elements, it only renders text.
* To output you use the directive `v-html`. This allows you to pass the name of the property which holds the HTML code. This directive tells Vue to render it as HTML instead of escaping it to text. Use this carefully as it does expose you to cross-site scriping attacks if used wrongly(user created content for example).

```
<p v-html="finishedLink"></p>
```

### Listening to events

* If `v-bind` allows you to bind something in your template and pass data to it, `v-on` is the opposite, it allows you to listen to an element and get data from it.
* `v-on` allows you to listen to any DOM event listed (mouseenter, mouseleave etc). For example `v-on:click="method"`.
* `v-on:click="functionName"` takes the method name or to be executed code as an argument.
* There is a default event object created by the DOM which holds information about the event, this object is passed automatically by Vuejs to any method you call using `v-on`.
* You can add parentheses to a called method and pass your own argument `v-on:click="functionName(arg)"`
* Using `$event` you can pass your own argument and the event object to a method `v-on:click="functionName(arg, $event)"`.

#### Event modifiers

* Event modifiers allows you to modify an event. For example here VueJs stops the propagation of the event. `v-on:mousemove.stop=""`.
* Other modifiers are `.prevent` for preventDefault.
* Modifiers can be chained `v-on:mousemove.stop.prevent=""`.

#### Key modifiers

* Key modifiers allow you to control on which key a method is fired `<input type="text" v-on:keyup.enter.space="alertMe">` these can also be chained.
* This enables us to listen to specific keys, these are only useable for key-events

#### Writing Javascript code in templates

* In all the places where you can access the Vue instance like `{{ value }}` or `v-on:click="function"` you can write Javascript code as long as it only has one expression and doesn't contain a loop or if statement.
* You can write simple Javascript in the template like this `{{ counter * 2 > 10 ? 'Greater than 10' : 'Smaller than 10'}}`.

#### Using Two-Way binding

* Two-way binding allows you to output data and listening to events at the same time.
* This can be done using the `v-model` directive

```
<div id="app">
  <button v-on:click="increase(2, $event)">Click me</button>
  <button v-on:click="counter++">Click me</button>
  <p>{{ counter * 2 > 10 ? 'Greater than 10' : 'Smaller than 10'}}</p>
  <p v-on:mousemove="updateCoordinates">
    Coordinated: {{ x }} / {{ y }}
    - <span v-on:mousemove.stop="">DEAD SPOT</span>
  </p>
  <input type="text" v-on:keyup.enter.space="alertMe">
</div>
```
```
new Vue({
	el: '#app',
  data: {
  	counter: 0,
    x: 0,
    y: 0
  },
  methods: {
  	increase: function(step, event) {
    	this.counter += step;
    },
    updateCoordinates: function(event) {
    	this.x = event.clientX;
      this.y = event.clientY;
    },
    alertMe: function() {
    	alert('Alert!');
    }
  }
});
```

## Section 3: Notes from SG Vue JS 

### Templates: 
 - describes structure and content of the app, aka the presentation layer (HTML)
 - instances describe what happens when a user interacts with the app (Javascript)

 ## Section 4: Notes from MS Vue JS
 ### Basics
  - 'this.foo' refers to anything inside the vue instance or component. Can be data, method, computed, ect.
  - vue creates the templates, renders them, and outputs the new html through the instance
  - data can be returned to the HTML or DOM with {{ foo }} where foo is the is the key in the data object.
  - methods are functions that can output thier returned value to the HTML or dom with {{ }}, returned values can be stored in the data object
    - example: a method that returns this.foo where foo is stored in data
  - can't use {{}} in HTML elements, instead use v-bind with the argument, for a link it would be v-bind:href="link" where link is stored in the data object, e.g. link: 'www.google.com'
  -using v-bind:(argument / html attribute) puts you in the vue instance, no need for {{}} 
  - v-once is used when you want to use the inital value of a data property even if it changes or updates later
  - vue escapes html elements, it's default is to render text, if you need to output a full html element, you can use v-html="foo" where foo is the data property / html you want rendered
  -v-on: is used to bind to something on the DOM (like a button) whereas v-bind is to bind data to the DOM
  - v-on: used for binding standard DOM events to the Vue instance.
  - add methods to update v-on:, again, this.foo references any data in the vue instance
  - $event is stored in vue as a default for events, can be called from the template when passed as an arguement in a methods function 
  ```
  data: {
    counter: 0,
    x: 0,
    y: 0
  },
  methods: {
    increase: function(step, event) {
      this.counter += step;
    }
    updateCoordinates: function(event) {
      this.x = event.clientX;
      this.y = event.clientY;
    }
  }
  <template>
    <div>
      <button v-on:click="increase(3, $event)">click</button>
      <p>{{counter}}</p>
      <p v-on:mousemove="updateCoordinates">{{x}}/{{y}}</p>
    </div>
  </template>
  ```
### Event Modifiers
  - event.stopPropogation() can be used to stop events from updating an HTML element
  - event.mousemove.stop can be used to stop propigation 
  - event.preventDefault() can be written as event.mousemove.prevent.
### Key Modifiers
  - .enter, .space, .tab 
  - ex v-on:keyup.enter.space="alertMe" will only throw alert when enter or space key is hit.

  - js can be written directly in template, referring to previous example: 
  ```
  <button v-on:click="counter++">click</button>
  ```
  would increase the counter stored in data by 1
  - two way binding: want to show data in template and if changed in template, update data property as well. v-model is used for two-way binding. example: 
  ```
  <template>
    <div id="app">
      <input type="text" v-model="name">
      <p>{{name}}</p>
    </div>
  </template>
  
  new Vue({
    el: '#app',
    data: {
      name: 'Ben'
    }
  })
  ```
### Reactivity
  - if a lot of different places depend on a data property, you can write the logic in the template and update with a new method. 
  - ex 
  ```
    <template>
    <div id="app">
      <button v-on:click="counter++"></button>
      <button v-on:click="counter--"></button>
      <p>{{counter}}</p>
      <p>{{result}}</p>
    </div>
  </template>
  
  new Vue({
    el: '#app',
    data: {
      counter: 0,
    }, 
    methods: {
      result: function () {
        return this.counter > 5 ? "greater" : "less"
      }
    }
  })
  ```
  - we can introduce the computed property of the vue instance that will only concern itself with the specfic data property referenced. 
  - the watch property can work to watch and only fire / react to specific changes. Best practice to use computed properites when you can ( more opitimized ). 
  - watch can be run async 
  ```
    <template>
    <div id="app">
      <button v-on:click="counter++">Increase</button>
      <button v-on:click="counter--">Decrease</button>
      <button v-on:click="secondCounter++">Second Counter</button>
      <p>{{counter}} | {{secondCounter}}</p>
      <p>{{result}} | {{output}}</p>
    </div>
  </template>
  
  new Vue({
    el: '#app',
    data: {
      counter: 0,
      secondCounter: 0
    }, 
    computed: {
      output: function() {
        return this.counter > 5 ? "greater" : "less"
      }
    },
    watch: {
      counter: function(value) {
        var vm = this;
        setTimeout(function() {
          vm.counter = 0;
        }, 2000)
      }
    },
    methods: {
      result: function () {
        return this.counter > 5 ? "greater" : "less"
      }
    }
  })
  ```
  - this is not stored in callbacks like in our watch function
  - : and @ can be used without the text before hand as a shortcut.
### Dynamic styling 
  - in data property, can add the property to attach css classes
  ```
  <div>
    <div>
    class="demo"
    @click="attachRed = !attachRed"
    :class="{red: attachRed}"
    </div>
  </div>

  new Vue({
    el: "#app",
    data: {
      attachRed: false
    }
  })
  ```
  - in :class pass an object where key is the css property (.red) and the value is the data property
  - the object could be written in a computed property instead of written in the template
    ```
  <div>
    <div>
    class="demo"
    @click="attachRed = !attachRed"
    :class="divClass"
    </div>
  </div>

  new Vue({
    el: "#app",
    data: {
      attachRed: false
    },
    computed: {
      divClass: function() {
        return {
          red: this.attachRed,
          blue: !this.attachRed
        }
      }
    }
  })
  ```
  - v-model can also be used to attach a class from a data property 
  ```
  <div>
    <div>
    class="demo"
    @click="attachRed = !attachRed"
    :class="divClass"
    </div>
    <div :class="[color, {red: attachRed}]></div>
    <input type="text" v-model="color">
  </div>

  new Vue({
    el: "#app",
    data: {
      attachRed: false,
      color: 'green' //refers to css class
    },
    computed: {
      divClass: function() {
        return {
          red: this.attachRed,
          blue: !this.attachRed
        }
      }
    }
  })
  ```
  - we are just outputting a class name from the input to the div, can also attach multiple classes in an array
  - again, css class of red will be determined by the attachRed value, computed by the divClass computed function...
  - we can also direcly interact with the style with :syle="{backgroundColor: color}"
    - this could be the result of an input, async task, ect...
  - vue will prefix styles to work in all browsers with :style

### Conditional Rendering
  - v-if="example" can be used attach or detach from the dom. 
  - v-else="example" will connect to the last v-if used in the template 
  - template doesn't get rendered in the dom so we can nest multiple elements inside the template 
  - if you want to hide instead of detach, you can use v-show="example", adds style="display: none", v-if is better performance-wise
  - v-for can be used to render lists with v-for="example in examples"
  ```
  <li v-for="(ingredient, i) in ingredients>{{ingredient}} | {{i}}</li>
  ```
  - to add the index, you can put () and the first arg is the element the second is the index
  - templates again can be used ot render multiple elements 
  - can also loop through arrays with objects or through objects 
  - v-for can be nested to loop through the key/value pairs in objects 
  ```
  <li v-for="person in persons">
    <div v-for="(value, key, index) in person"></div>
  </li>
  ```
  - () can be used to loop through the key value pairs (properties) in the data object and the index. NOTE: the value must come first, then the key ("Brad", "name"), then the index.
  - for numbers you can just run a simple v-for="number in 10" if number is not a data property, vue will loop through the integers in that range
  - vue proxies the .push() property
  - vue will update the position but not the element in that position. to be safe add :key="person" to add unique key in for loops



