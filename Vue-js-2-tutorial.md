# Vue.js 2 tutorial by The Net Ninja
## 1. Introduction
### What is Vue.js?
- A front-end framework (similar to React or Angular)
- Create javascript-driven applications for the web
- Runs in the browser
- No need to make multiple server requests for pages

### Why Vue.js?
- Very lean (16kb)
- Very high run-time performance

## 2. The Vue Instance
The first thing we tend to do when we start writing Vue.js code is creating a Vue instance:

    `new Vue({});`

Its role is to control a certain part of our application. We could have an app running through a single Vue instance or several of them.

### How does this instance control the app?
Inside the Vue instance we're going to pass through some parameters and methods:

```javascript
new Vue({
    el: '#vue-app', // Lists the elements this instance is going to control. We use # to identify vue-app as an ID
    data: { // Object that contains a list of key:pair values with the information the app contains
        name: 'Shaun'
    }
});
```

## 3. Data & Methods
We can also include as many methods as we want within the Vue instance:

```javascript
new Vue({
    el: '#vue-app',
    data: {
        name: 'Shaun',
        job: 'Ninja'
    },
    methods:{
        greet: function(time){ // We need to return something that can be printed out to the DOM (e.g. a String)
            return 'Good Morning'
        }
    }
});
```

Then, if we wanted to call this function from our html template:

```html
<div id="vue-app">
    <h1>{{ greet('afternoon') }}</h1>
</div>
```

Within these functions, we can also access the data in the Vue instance without having to do lots of attribute "chaining". So, from within the instance we would retrieve the `job` attribute like this: `this.job`, and not like this: `this.data.job`.

## 4. Data Binding
If we want to bind our data to any kind of attribute (like `href`, for instance), we can use directives. In this specific case, we would use the v-bind directive:

```html
<div id="vue-app">
    <h1>Data Binding</h1>
    <a v-bind:href="website">The Net Ninja</a>
    <input type="text" v-bind:value="name"/>
    <p v-html="websiteTag"></p>
    <!-- we could also just use ":" and omit "v-bind" and it would still work-->
</div>
```

We just need to provide the directive with the name of the attribute we want to bind (in this case, `website`):

```javascript
new Vue({
    el: '#vue-app',
    data: {
        name: 'Shaun',
        job: 'Ninja',
        website: 'http://www.thenetninja.co.uk',
        websiteTag: '<a href="http://www.thenetninja.co.uk">The Net Ninja Website</a>'
    }
    // ...
});
```

We can also use directives to achieve more complicated tasks, for instance printing a list of names:

```html
<h1>Favorite food</h1>
<ul>
    <li v-for="item in food" v-text="item"></li> <!-- This is one way of doing it, or you may also use {{ item }} -->
</ul>
```

```javascript
new Vue({
    el: '#vue-app', // Lists the elements this instance is going to control. We use # to identify vue-app as an ID
    data: {
        age: 25,
        x: 0,
        y: 0,
        food: ['Spaghetti', 'Banana', 'Orange', 'Tomato']
    },
    methods: {
        // ...
    }
});
```

## 5. Events
We can make Vue.js respond to specific events. For instance, if we want to add 2 buttons that add/subtract 1 to `age`:

Age is 25:
```javascript
new Vue({
    el: '#vue-app', // Lists the elements this instance is going to control. We use # to identify vue-app as an ID
    data: {
        age: 25
    },
    methods:{
        add: function(inc){
            this.age += inc;
        },
        subtract: function(dec){
            this.age -= dec;
        },
        updateXY: function(event){
            // Use console.log(event)
            this.x = event.offsetX;
            this.y = event.offsetY;
        }
    }
});
```

```html
<div id="vue-app">
    <h1>Events</h1>
    <!-- We can either access the attribute directly... -->
    <button @click="age++">Add a year</button>
    <button v-on:click="age--">Subtract a year</button>
    <!-- ... or we can call a method. -->
    <button v-on:dblclick="add(10)">Add 10 years</button>
    <button @dblclick="subtract(10)">Subtract 10 years</button>
    <p>My age is {{ age }}</p>
    <div id="canvas" v-on:mousemove="updateXY">{{ x }}, {{ y }}</div>
</div>
```

TO output a coordinate I need access to the event object. Whenever we use a DOM event we need access to the event object.

## 6. Event Modifiers
Event modifiers allow to complement events with added functionality.

Following the previous example, if we wanted to just fire an event once, we could use a special modifier to do so:

```html
<div id="vue-app">
    <!-- ... -->
    <button v-on:click.once="add(1)">Add a year</button>
    <!-- ... -->
</div>
```

Or we could prevent the default behavior of an event from firing:

```html
<div id="vue-app">
    <!-- ... -->
    <a v-on:click.prevent="click" href="http://www.thenetninja.co.uk">The
    <!-- ... -->
</div>
```

## 7. Keyboard Events
Keyboard events work just like the mouse events we've seen before:

```html
<div id="vue-app">
    <h1>Keyboard Events</h1>
    <label>Name:</label>
    <input type="text" v-on:keyup.enter="logName" />
    <label>Age:</label>
    <input type="text" v-on:keyup.enter="logAge" />
</div>
```

```javascript
new Vue({
    el: '#vue-app',
    data: {

    },
    methods:{
        logName: function(){
            console.log('you entered your name')
        },
        logAge: function(){
            console.log('you entered your age')
        }
    }
});
```

We can stack these modifiers too. If we wanted the user to press `Alt` + `Enter` to fire the event, we could chain them like this: `v-on:keyup.alt.enter`

## 8. Two-Way data binding
We can achieve two-way data binding by associating HTML elements to the variables in our Vue instance with `v-model`. This immediately updates the variable if any changes are made to it, be it via the console or through the HTML element it is linked to:

```html
<div id="vue-app">
    <h1>Keyboard Events</h1>
    <label>Name:</label>
    <input type="text" v-model="name" />
    <span>{{ name }}</span>
    <label>Age:</label>
    <input type="text" v-model="age" />
    <span>{{ age }}</span>
</div>
```

```javascript
new Vue({
    el: '#vue-app',
    data: {
        name: '',
        age: ''
    }
});
```

## 9. Computed properties
If you call 2 methods from the HTML file, both will run even though only a portion of the data changed (and thus there was no need to re-run everything). See the following example:

```html
<div id="vue-app">
    <h1>Computed Properties</h1>
    <button v-on:click="a++">Add to A</button>
    <button v-on:click="b++">Add to B</button>
    <p>A - {{ a }}</p>
    <p>B - {{ b }}</p>
    <p>Age + A = {{ addToA() }}</p>
    <p>Age + B = {{ addToB() }}</p>
</div>
```

```javascript
new Vue({
    el: '#vue-app', // Lists the elements this instance is going to control. We use # to identify vue-app as an ID
    data: {
        a: 0,
        b: 0,
        age: 20
    },
    methods: {
        addToA() {
            return this.a + this.age
        },
        addToB() {
            return this.b + this.age
        }
    }
});
```

If we click either of the two buttons, both `addToA()` and `addToB()` will run, even though only the value for either A or B was changed.

Using computed properties, all of them run on startup, and then they are only called when the underlying data is modified. They "watch" the variables and only run when needed. To implement computed properties, we will only need to change a few things:

```html
<div id="vue-app">
    <h1>Computed Properties</h1>
    <button v-on:click="a++">Add to A</button>
    <button v-on:click="b++">Add to B</button>
    <p>A - {{ a }}</p>
    <p>B - {{ b }}</p>
    <p>Age + A = {{ addToA }}</p> <!-- Remove parentheses -->
    <p>Age + B = {{ addToB }}</p>
</div>
```

```javascript
new Vue({
    el: '#vue-app',
    data: {
        a: 0,
        b: 0,
        age: 20
    },
    computed { // Move the functions from the 'methods' object to 'computed'
        addToA() {
            return this.a + this.age
        },
        addToB() {
            return this.b + this.age
        }
    }
});
```

## 10. Dynamic CSS classes
We can also apply dynamic CSS classes to our elements using data binding. In this example, we style the HTML elements according to the values of some properties of our Vue instance:

```html
<div id="vue-app">
    <h1>Dynamic CSS</h1>
    <h2>Example 1</h2>
    <div v-on:click="available = !available" v-bind:class="{available: available}"> <!-- Curly braces because it expects an object -->
        <span>Ryu</span>
    </div>
    <h2>Example 2</h2>
    <button v-on:click="nearby = !nearby">Toggle nearby</button>
    <button v-on:click="available = !available">Toggle available</button>
    <div v-bind:class="compClasses">
        <span>Ryu</span>
    </div>
</div>
```

```javascript
new Vue({
    el: '#vue-app',
    data: {
        available: false,
        nearby: false
    },
    computed: {
        compClasses() {
            return { // v-bind:class expects an object, so we return an object
                available: this.available,
                nearby: this.nearby
            }
        }
    }
});
```

For this specific example, this CSS styling is used:

```css
span{
    background: red;
    display: inline-block;
    padding: 10px;
    color: #fff;
    margin: 10px 0;
}

.available span{
    background: green;
}

.nearby span:after{
    content: "nearby";
    margin-left: 10px;
}
```

## 11. Conditionals
There are 2 directives: `v-if` and `v-show`. `v-if` has the ability of removing an element entirely from the DOM, whereas v-show will just set the property `display: none`:

```html
<div id="vue-app">
    <h1>Conditionals</h1>
    <button v-on:click="error = !error">Toggle Error</button>
    <button v-on:click="success = !success">Toggle Success</button>
    <p v-if="error">There has been an error</p>
    <p v-else-if="success">Whooo success! :)</p>
    <p v-show="error">There has been an error</p>
    <p v-show="success">Whooo success! :)</p>
</div>
```

```javascript
new Vue({
    el: '#vue-app',
    data: {
        error: false,
        success: false
    },
    computed: {

    }
});
```

## 12. Looping with `v-for`
We have several ways of dealing with for loops, and some additional utilities that we have not discussed previously.

We can retrieve the position of an element in an array and output it as a list or even loop through `<div>` (or `<template>`) tags to create several of them and populate them with the data from the instance. By using `<template>` instead of `<div>`, we make the markup much clearer, since it will use what lies inside the template and print it to the browser without creating unnecessary `<div>` elements:

```html
<div id="vue-app">
    <h1>Looping through lists</h1>
    <ul>
        <li v-for="(ninja, index) in ninjas">{{ index }} . {{ ninja.name }} - {{ ninja.age }}</li>
    </ul>
    <template v-for="(ninja, index) in ninjas">
        <h3>{{ index }} . {{ ninja.name }}</h3>
        <p>{{ ninja.age }}</p>
    </template>
</div>
```

```javascript
new Vue({
    el: '#vue-app',
    data: {
        ninjas: [
            {name: 'Ryu', age: 25},
            {name: 'Yoshi', age: 35},
            {name: 'Ken', age: 55}
        ]
    }
});
```

## 14. Multiple Vue instances
We can use multiple Vue instances the way we would use a single instance. However, it is important to assign variable names to the Vue instances when working with more than just one, since it makes it much more comfortable to access data from either object.

```javascript
var instance_one = new Vue({
    el: '#vue-app-one',
    data: {
        property1: 'Some Value'
    }
    // ...
});

var instance_two = new Vue({
    el: '#vue-app-two',
    // ...
    methods: {
        get_property1: function() {
            return instance_one.property1
        }
    }
});
```

## 15. Intro to Components
Components are reusable pieces of code/functionality/template you can use in different Vue instances. The component takes 2 arguments:

1. Component name
2. Properties object

We have omitted the code for `vue-app-one` and `vue-app-two`, since they are basically an empty Vue instance:

```javascript
Vue.component('greeting', { // Second argument begins here:
    template: '<p>Hey there, I am {{ name }} . <button v-on:click="changeName">Change name</button></p>',
    data: function() {
        return {
            name: 'Yoshi'
        }
    },
    methods: {
        changeName: function() {
            this.name = 'Mario'
        }
    }
});
```
```html
<h1>Components</h1>
<div id="vue-app-one">
    <h2>Vue app one</h2>
    <greeting></greeting>
</div>
<div id="vue-app-two">
    <h2>Vue app two</h2>
    <greeting></greeting>
</div>
```

Note that `data` is no longer an object, but a `function`. If we used an object, all the Vue instances that used it would share the reference to that object and thus, whenever the information in the `component` changed, this change would be reflected across all Vue instances.

## 16. Refs
Vue lets us obtain data from the DOM and access information such as text input values, inner HTML, etc.

`this.$refs` returns an object with the `key:value` pairs of refs the Vue instance can access within its respective `<div>` in the DOM:

```html
<div id="vue-app">
    <h2>Refs</h2>
    <input type="text" ref="input"/>
    <button v-on:click="readRefs">Submit</button>
    <p>Your fav food: {{ output }}</p>
</div>
```

```javascript
new Vue({
    el: '#vue-app',
    data: {
        output: 'Your fav food'
    },
    methods: {
        readRefs: function() {
            console.log(this.$refs.input.value);
            this.output = this.$refs.input.value;
        }
    }
});
```

## 17. The Vue CLI

Create a dev environment workflow with webpack:
- ES6 features
- Compile & minify our JS into 1 file
- Use single file templates
- Compile everything on our machine, not in a browser
- Live reload dev server

## 18. Vue files & The root component
Components can also be contained in their own .vue files, instead of being created through the Vue.component() property.

In a template, **we need to have 1 root element** between the <template> tags (for instance, a <div> element).

## 19. Nesting Components
There are 2 ways of registering a component: globally or locally.

When we're nesting components we have to register the component for use. If we register the component globally, it means we can nest that component into any other component. If we register it locally, it means we can only use that component nested into the component we register it locally with.

### GLobally:

1. Define the component:
```html
<!-- Component definition -->
<template>
  <ul>
      <li v-for="ninja in ninjas">{{ ninja }}</li>
  </ul>
</template>

<script>
export default {
  data () {
    return {
      ninjas: ['Yoshi', 'Mario', 'Ryu']
    }
  }
}
</script>
```

2. Import it into `main.js`:
```javascript
import Vue from 'vue'
import App from './App.vue'
import Ninjas from './Ninjas.vue'

Vue.component('ninjas', Ninjas)

new Vue({
  el: '#app',
  render: h => h(App)
})
```

3. Nest it within the root component:
```html
<template>
  <div>
      <h1>{{ title }}</h1>
      <ninjas></ninjas>
  </div>
</template>
```

### Locally:

1. Define the component:
Component definition works like in the global nesting methodology; only how we nest the object varies.

2. No `main.js` import:
```javascript
import Vue from 'vue'
import App from './App.vue'

new Vue({
  el: '#app',
  render: h => h(App)
})
```

3. Nest it within App component:
```html
<template>
  <div>
      <h1>{{ title }}</h1>
      <ninjas></ninjas>
  </div>
</template>

<script>
import Ninjas from './Ninjas.vue' // Import here!!

export default {
  components: {
      'ninjas': Ninjas
  },
  data () {
    return {
      title: 'Ninja App'
    }
  }
}
</script>
```

## 20. Component CSS
Careful when CSS styling components, since styles applied to either the parent or the nested component may override styles defined elsewhere in the component tree.

In order to apply styles just to the local component, we can use the `scoped` option along with the `<style>` tag:

```html
<style scoped>
h1{
    color: purple;
}
</style>
```

## 22. Props
Props are ways to pass data from one component to another (parent to child).

In the nested component, we have to receive the content from the parent with the `prop` element. We may access the `prop` from the child just like we access data from within the component:
```html
<script>
export default {
    props: ['ninjas'],
    data () {
        return {

        }
    },
    methods: {
        test() {
            console.print(this.ninjas)
        }
    }
}
</script>
```

From the parent, we then pass the information using the `ninjas` prop within the component tag, along with a `v-bind` directive:

```html
<template>
  <div>
    <app-ninjas v-bind:ninjas="ninjas"></app-ninjas>
  </div>
</template>
```
