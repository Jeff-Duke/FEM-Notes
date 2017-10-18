## Why Vue
* Slightly better performance than React, no use of polyfills
* Elegant, easy to use, fun to use

## Directives & Data Rendering
* Directives are super powerful
* the data object is what we use to manage state

### Vanilla JS vs Vue

```
const items = [
  'thingie',
  'another thing',
  'ye olde stuff',
  'and another thing'
];

function listOfStuff() {
  let full_list = '';
  for (let i = 0; i < items.length; i++) {
    full_list = full_list + `<li> ${items[i]}</li>`
  }
  const contain = document.querySelector('#container');
  contain.innerHTML = `<ul> ${full_list} <ul>`;
}
```

#### Vue version:

```
new Vue({
  el: '#app',
  data: {
    items: [
      'thingie',
      'another thing',
      'ye olde stuff',
      'and another thing'
    ]
  }
});

<!-- HTML -->

<div id="app">
  <ul>
    <li v-for="item in items">
      {{ item }}
    </li>
  </ul>
</div>
```

Here we're just giving the computer what we want to have done and the computer does its thing.

Vue is:
* clean
* semantic
* declarative
* legible
* easy to maintain
* reactive


## Directives

Tiny pieces of html that Vue gives us, some bits of functionality to add to our markup.

```
v-text      v-on
v-html      v-bind
v-show      v-model
v-if        v-pre
v-else      v-cloak
v-else-if   v-once
v-for
```
You can also make custom directives!!!

### V-for:
* loops through a set of values, a list, a static number (num in 5 etc).

### v-model:
* Creates a relationship between the data in the instance/component and a form input, so you can dynamically update values
```
new Vue({
  el: '#app',
  data() {
    return {
      message: 'This is a good place to type things.'
    }
  }
});

<div id="app">
  <h3>Type here: </h3>
  <textarea v-model="message" class="message" rows="5" maxlength="72"/> //this has a 2-way binding with data
  <br>
  <p class="booktext">{{ message }}</p>
</div>

```
* You can use an object for the data, but it's better to write it as a function and return that data.
  * When you later want to break things into components, you want each component to have its own isolated scope, you have to have a function to do that

* We can also use v-model for things like checkboxes:
```
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>

new Vue({
  el: '#app'
  data: {
    checkedNames: [], //we're just pushing data into the array based on the v-model
  }
});
```
How you could create that dynamically:

```
<span v-for="option in options">
  <input type="checkbox" :id="option.value" :value="option.value" v-model="checkedNames">
  <label for="option.value">{{ option.value}}</label>
</span>

new Vue({
  el: '#app'
  data: {
    checkedNames: [],
    options: [
      { value: 'John' },
      { value: 'Paul' },
      { value: 'George' },
      { value: 'Ringo' },
    ]
  }
});
```

* There are modifiers available for the directives:
  * v-model.number - converts to number
  * v-moled.trim - trims the whitespace
  * v-model.lazy - won't populate the content automatically. it will wait to bind until an event happens. (it listens to change events instead of input)

### v-if/v-show
* is a conditional that will display information depending on meeting a requirement. this can be anything - buttons, forms, divs, components

```
<h3>What is your favorite taco?</h3>
<textarea v-model="tacos"></textarea>

<br>

<button type="submit v-if="tacos"">Let us know!</button>

new Vue ({
  el: '#app',
  data() {
    return {
      tacos: ''
    }
  }
});
```
* V-if is unmounting/remounting the element completely

### v-show
* same functionality as `v-if` but the element exists on the DOM, toggling the visibility
  * use where someone might hide/show a bunch, bad performance to unmount/remount

* use `v-if` if you're doing a big block of content.

### v-if/v-else
* conditionally render one thing or another. there's also v-else-if
  * no more ternary! ;)

```
<div v-if="tacos">
  <p v-if="tacos === 'yes'">👍</p>
  <p v-else>you're a monster</p>
</div>
```

### v-bind or :
* one of the most useful directives so that's why there's the : shortcut.  use it for:
  * class
  * style bindings
  * id's
  * creating dynamic props, etc...

```
  <h3>What's your favorite taco?</h3>
  <textarea v-model="tacos"/>

  <br>
  <button :class="[tacos ? activeClass : '']"></button> //wonder if you could use && there instead of ternary...

  new Vue({
    el: "#app",
    data() {
      return {
        tacos: '',
        activeClass: 'active'
      }
    }
  });
```

### Interactive style bindings
```
<div id="contain"  :style="{ perspectiveOrigin: `${x}% ${y}%` }"> </div>
```
* we could skip making a class for something and just get to adding some inline styles to it right there

### v-once and v-pre
* v-once will not update once it's been rendered.
* v-pre will print out the inner text exactly how it is, including code(good for documentation). so it'll show the mustache tag instead of what it's referencing

* the `$` is something special from the vue instance

### v-on or @
* Extremely useful, this is your even binder like @click or v-on="keyup"
  * able to pass in a parameter for the event like (e)
  * can also use ternaries directly

*For example, like an e-commerce store, you want people to add qty but not subtract below 0:

```
new Vue({
  el: '#app',
  data() {
    return {
      counter: 0
    }
  }
});

<button @click="counter > 0 ? counter -= 1 : 0">remove</button>
<button @click="counter += 1">add</button>
```
* Good for doing small amount of logic
* we can also do multiple bindings:
```
<div @="
  click: onClick,
  keyup: onKeyup,
  keydown: onKeydown
"></div>
```
* useful for like desktop needing an onclick but mobile needing an onPress or something like that
  * or a hover on desktop and a touch event on mobile

### Modifiers
* `@mousemove.stop` is comparable to `e.stopPropogation()`
* `@mousemove.prevent` is like `e.preventDefault()`
* `@submit.prevent` this will no longer reload the page on submission
* `@click.once` not to be confused with v-once, this _click event_ will be triggered once.
* `@click.native` so that you can listen to native events in the DOM
* there are keycodes available and you can create your own modifiers!!

### v-html
* Great for strings that have html elements that need to be rendered
```
new Vue({
  el: "#app",
  data() {
    return {
      tacos: `I like <a href="tacoplace.com">Al Pastor</a> tacos`
    }
  }
});

<div v-html="tacos"></div>
```
* This will render an inline link instead of rendering the code for the `<a>` tag.
* Don't use this for anything user-generated where a user might be able to inject a cross-site script etc

### v-text
* pretty much the same as the mustache template
* if you want to dynamically update, it's recommended to use mustach templates instead
  * so maybe just always use mustache

*Check out the CSS-Tricks Vue Guide*



## Methods, Computed, Watchers
* Methods we'll use a ton
* Computed we'll use a lot
* Watchers on special occasion

### Methods
* Are bound to the Vue instance, they are useful for functions you would like to access in directives
```
new Vue ({
  el: '#app',
  data() {
    return {
      counter: 0,
    }
  },
  methods: {
    increment() {
      this.counter++;
    },
    decrement() {
      this.counter --;
    },
  }
})
```
* Any time we use methods, computed, watchers you can use `this` like `this.counter` `this.whatever`
* Don't use an arrow function for the method because we won't bind it properly
* Can use arrow functions within the method
* Need to be careful about binding `this` properly

#### Another method example:
```
new Vue({
  el: '#app',
  data: {
    newComment: '',
    comments: [
      'Looks great!',
      'I love that!!'
    ]
  },
  methods: {
    addComment() {
      this.comments.push(this.newComment);
      this.newComment = '';
    }
  },
});

<ul>
  <li v-for="comment in comments">
    {{ comment }}
  </li>
</ul>
<input
  v-model="newComment"
  @keyp.enter="addComment"
  placehodler="Add a comment"
/>
```
* Here we're filling out that comment field as the user types, when they submit, we call the add comment method that takes it out of the `newComment` key and pushes it to the `comments` array.

#### Demo using axios
```
<form @submit.prevent="submitForm">
  <div>
    <label for="name">Name:</label>
    <input id="name" type="text" v-model="name" required>
  </div>
  <div>
    <label for="email">email:</label>
    <input id="email" type="text" v-model="email" required>
  </div>
  <div>
    <label for="caps">caps:</label>
    <input id="caps" type="text" v-model="caps" required>
  </div>
  <button :class="[name && activeClass]">Submit</button>
</form>

new Vue({
  el: '#app',
  data() {
    return {
      userId: 1,
      name: '',
      email: '',
      caps: '',
      response: '',
      activeClass: 'active',
    }
  },
  methods: {
    submitForm() {
      axios.post('//jsonplaceholder.typicode.com/posts', {
        userID: this.userID,
        name: this.name,
        email: this.email,
        caps: this.caps
      }).then(response => {
        this.response = JSON.stringify(response, null, 2)
      }).catch(error => {
        this.response = 'Error: ' + error.response.status
      })
    }
  }
});
```

### Computed Properties
* Calculations that will be cached and will only be updated when needed
  * highly performant, use with understanding
* Use as a property
* Different view of the same data
* Doesn't affect the first piece of data

Computed:
* Runs only when a dependancy has changed
* Cached
* Should be used as a property, in place of data\
* Getter only, but you can define a setter
* Can still use methods on the computed property

Methods:
* Runs whenever an update occurs
* Not cached
* Typically invoked from v-on/@, but flexible
* Getter/Setter

### Watchers
* What is reactive? What does reactive mean?
  * Reactive programming is programming with async data streams
    * A stream is a sequence of ongoing events ordered in tim ethat offer some hooks with which to observe it.
    * Think about a transition on a hover state, the moment it's hovered on, the stream while it transitions and then its final state
  * When we use reactive premises for building applications, this means it's very easy to update state in reaction to events.
  * Angular has dirty checking
  * Cycle and Angular 2 use reactive streams like XStream and Rx.js
  * Vue.js, MobX or Ractive.js all use a variation of getters/setters.
  * React is not Reactive :( it uses a pull approach rather than a push approach
  * In Vue the vue instance walks through the properties and converst them to getter/setters
    * In the vue instance, the data object is super important
      * Vue cannot detect property addition or deletion so we create the object to keep track

* Each component has a watcher instance
* The props touched by the wather are registered as dependencies
* When the setter is trggered, it lets the watcher  know and causes a rerender
* Using a virtual DOM -- Calcs are cheap, reaching into the DOM is expensive
* Things need to exist in the Data model before the page loads, anything added later is just a prop on the object, doesn't have the watcher/getters/setters
* The Vue instance is the middle man between the DOM and the business logic
* Watch update the DOM only if it's required
* Only updates the DOM for things that need to change so its faster
* *Watchers* are good for async updates/transitions
* Watchers give you access to the new value and the old value
  * Useful for keying off animations etc
* We can also gain access to nested values with `deep`
```
watch: {
  watchedProp {
    deep: true,
    nestedWatchedProperty (value, oldValue) {
      //that hawtness
    }
  }
}
```
