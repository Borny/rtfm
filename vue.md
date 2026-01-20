# VueJS

- [Set up](#set-up)
- [Life Cycles](#life-cycles)
- [Interpolation](#interpolation)
- [Binding](#binding)
- [Event Binding](#event-binding)
- [Two-Way Binding](#two-way-binding)
- [Computed Properties](#computed-properties)
- [Watchers](#watchers)
- [Dynamic styling](#dynamic-styling)
- [Conditional rendering](#conditional-rendering)
- [Lists](#lists)
- [Vue files](#vue-files)
- [Components](#components)
- [Asynchronous Components](#asynchronous-components)
- [Composition API](#composition-api)
- [Scoped styles](#scoped-styles)
- [Forms](#forms)
- [Routing](#routing)
- [Animations](#animations)
- [Vuex](#vuex)

## Set up

- Web App
- Vue App

### Web App

- Import Vue script
- HTML
- JS

#### Import Vue script

Import the **vuejs** script in the index.html file **before** the js file.
`<script src="https://unpkg.com/vue@next" defer></script>`

#### JS

```javascript
// Use the createApp to initialize a Vue app
const app = Vue.createApp({
  template: ` // will display the HTML code
    <div>Content...</div>
  `
  },
  data() {
    // will return the properties used in the template
    return {
      name: 'Mat Timolike',
      age: 38,
      message: 'awesome',
    };
  },
  methods: {
    // will list all the methods inside the Vue instance
    computeAge() {
      return this.age + 5;
    },
    randomNumber() {
      return Math.random();
    },
  },
});

// In order for the Vue instance to work, it needs to be connected to an HTML element
// That element and all its children can be accessed by the Vue instance. It can be any CSS selector
// The HTML section targeted by the mount method becomes the "Template" of the application
app.mount('#assignment');
```

#### HTML

```html
<body>
  <section id="assignment">
    <h2>{{ name }}</h2>
    <p>{{computeAge()}}</p>
    <p v-bind:title="message">{{randomNumber()}}</p>
    <div>
      <img v-bind:src="randomImage" />
    </div>
    <input type="text" v-bind:value="name" />
  </section>
</body>
```

### Vue App

- Vue CLI
- Create a Vue App
- Running the App

#### Vue CLI

Install the Vue CLI:
`npm i -g @vue/cli`

#### Create a Vue App

`vue create name-of-the-app`

#### Running the App

`npm run serve`

## Life Cycles

- createApp
- beforeCreate
- created
- beforeMount
- mounted
- beforeUpdate
- updated
- beforeUnmount
- unmounted

## Interpolation

Displaying the properties stored in the **data** method in the HTML tags using **interpolation** : {{ }}.

```javascript
data() {
    // will return the properties used in the template
    return {
      propertyToDisplay: 'Whatever you want to show !',
    };
  },
```

`<p>{{ propertyToDisplay }}</p>`

## Binding

Binding the properties to the HTML template

- v-bind
- v-link
- v-html
- ref

```javascript
data() {
    // will return the properties used in the template
    return {
      imageSRC: 'https://www.unsplash.com/dfmlkjqmflkjdsf',
      defaultInputValue: 'default input value',
      linkProperty: 'https://www.unsplash.com/dfmlkjqmflkjdsf',
      htmlProperty: '<span>Whatever text you want</span>',
    };
  },
```

### v-bind

`<img v-bind:src="imageSRC" />`  
`<input v-bind:value="defaultInputValue" />`  
or use the **shorthand** version:  
`<img :src="imageSRC" />`  
`<input :value="defaultInputValue" />`

### v-link

`<a v-link:href="linkProperty">Click the link</a>`

### v-html

`<p v-html="htmlProperty"></p>`
This should be used with caution as it can lead to security issues like XSS attacks

### ref

**ref** can be used to retrieve values just like **v-bind**.

`<input type="text" ref="someRef"/>` // ref is given a value that can be accessed in the JS file

```javascript
methods: {
  getRef(){
    console.log(this.$ref.someRef.value) // the $ref object will hold all the ref values of the template
  }
}
```

## Event Binding

- v-on:click || @click
- v-on:input
- v-on:once
- Modifiers

Methods are used with **event binding**. They shouldn't be used with data binding as they will be called each time some data changes in the template which will result in bad performance.

### v-on:click

`<button v-on:click="methodToCall">Click me</button>`
**shorthand** version:
`<button @click="methodToCall">Click me</button>`

### v-on:input

```html
<input type="text" v-on:input="methodToCall" />
<p>{{ enteredInput }}</p>
```

```javascript
date(){
  return {
    enteredInput: ''
  }
},
methods: {
    enteredInput(event) {
      this.enteredInput = event.target.value
    }
  },
```

### v-once

Will kepp the initial value of a property

```html
<input type="text" v-on:input="methodToCall" />
<p v-once>{{ enteredInput }}</p>
```

### Modifiers

Modifiers add options to the events:

- .stop // the click event's propagation will be stopped
- .prevent // the submit event will no longer reload the page
- .capture
- .self // only trigger handler if event.target is the element itself
- .once
- .passive

Mouse modifiers:

- .left
- .right
- .middle

Input modifiers:

- lazy
- number
- trim

```html
<input type="text" v-on:keyup="methodToCall" /> // will only fire the method
when a key is pressed
<p>{{ enteredInput }}</p>

<input type="text" v-on:keyup.enter="methodToCall" /> // will only fire the
method when the 'enter' key is pressed
<p>{{ enteredInput }}</p>
```

## Two-Way Binding

- v-model

### v-model

**v-model** allows for two-way data-binding. It is a shortcut for **v-on:input** combined with **v-bind:value**:

```html
<input type="text" v-model="propertyToUpdate" />
// same as
<input type="text" v-on:input="methodToCall" v-bind:value="" />
<button v-on:click="updateEvent">Update</button>
<p>{{ propertyToUpdate }}</p>
```

## Computed Properties

Methods should not be used in the DOM due to performance issues (just like in Angular) as Vue will run any methods present in the HTML code when the template is rerendered.
Avoid placing logic in the template, instead use **Computed Properties**. They are used to handle data that depend on other data.

```html
<input type="text" v-model="name" />
<p>{{ computedProperty }}</p>
// the computed property is a method but is called like a property in the
template
```

```javascript
const app = Vue.createApp({
  // used for properties
  data() {
    return {
      name: "",
    };
  },
  // used for changing certain properties
  computed: {
    computedProperty() {
      if (this.name === "") {
        return (this.name = "");
      }
      return `Your name is ${this.name}`;
    },
  },
  methods: {}, // used for events
});
```

## Watchers

Watchers are used to watch property/data changes. They are usefull for _behind the scene_ actions.
They are not referenced in the template. They are only present in the JS file.

```javascript
const app = Vue.createApp({
  // used for properties
  data() {
    return {
      name: "",
    };
  },
  watch: {
    name(value) {
      // will watch the changes on the property 'name'
      console.log(value);
      // it can be used to trigger a method based on any condition
      if (value === "Peter") {
        // run any existing methods
      }
    },
  },
  methods: {}, // used for events
});
```

## Dynamic styling

- Inline style
- classes

### Inline style

**:style** can be used to add inline style dynamically :

`<div class="demo" :style="{borderColor: boxAselected ? 'red' : 'yellow'}" @click="selectBox('A')"></div>`

### Classes

**:class** can be used to add CSS classes dynamically :

`<div class="demo" :class="{someClass : conditionToEvaluate, randomClass: true}" @click="selectBox('A')"></div>`
Array syntax:
`<div :class="['demo', {someClass : conditionToEvaluate, randomClass: true}]" @click="selectBox('A')"></div>`

## Conditional rendering

- v-if / v-else-if / v-else
- v-show

### v-if / v-else-if / v-else

This will **remove** or **add** an element from the DOM depending on a condition.

`<p v-if="true">This paragraph will be rendered in the DOM</p>`
`<p v-else>This paragraph will not be rendered in the DOM</p>`

`<p v-if="array.length">This paragraph will be rendered in the DOM if the aray length is superior to 0</p>`
`<p v-else-if="!array.length">This paragraph will be rendered in the DOM if the array length is equal to 0</p>`
!Important => `v-else` should appear in the tag directly after the tag that has the `v-if` directive

### v-show

**v-show** does essentially the same as **v-if** but doesn't have an _else_ option and doesn't **remove** or **add** the elements from the DOM but **toggles** the **display** property to _none_

`<p v-show="false">This paragraph will be rendered in the DOM but hidden with 'display=none'</p>`

## Lists

- v-for

### v-for

**v-for** can be used to render lists in the template. It accepts arrays, numbers and objects.
It **will not rerendered** the entire list when an item is removed/added to the DOM, which could lead to performance issues.

`goals = [ 'Complete the course', 'Master Vue', 'Build an awesome app with it !']`

`<ul><li v-for="(goal, idx) in goals" :key="goal">{{ idx + 1 }} - {{ goal }}</li></ul>`
`<ul><li v-for="num in 10" :key="num">{{num}}</li></ul>`
`<ul><li v-for="(value, key, idx) in {name: 'Peter',aka: 'Spiderman'}" :key="key">{{idx}} - {{key}} : {{value}}</li></ul>`

!Important => the **key** attribute should always be used with _v-for_ as it will prevent unexpected behavior when adding/removing items from the list. It should be an unique identifier, usually the _id_ of the item.

## Vue files

A **Vue file** can only be used inside a Vue App, as Vue will compile the all the files into regular HTML, CSS and JS files so that the browser can read them.

- standard Vue file
- importing the Vue files

### Vue file structure

```html
<template></template> // will hold the HTML code
<script></script>
// will hold the Javascript code <style></style> // will have the styling
```

### importing the Vue files

The Vue files need to be imported into the main JS file

```html file - (i.e: App.vue)
<template></template>
<script>
  export default{
    data() {},
    methods: {},
    ...
  }
</script>
<style></style>
```

```javascript
import { createApp } from "vue"; // allow for the creation of the app
import App from "./App.vue"; // Main Vue file
const app = createApp(App);
app.mount("#app"); // selector generated by the Vue CLI present in the index.html (main and only HTML file in the Vue App)
```

## Components

A Component is a Vue file that can be imported in other Vue files.

- Importing a component globally
- Importing a component locally
- Props
- Custom Events
- Provide/Inject

### Importing a component globally

Here the **FriendContact** component will be available _globally_, it can then be used in any component of the application. But it will also always be downloaded when the application starts.

In the **main.js**

```javascript
import { createApp } from "vue";
import App from "./App.vue";
import FriendContact from "./components/FriendContact.vue";
const app = createApp(App);
// The "component" method needs a string that will be used as the HTML tag and the name of the Vue file to import
// The HTML tag needs to be in the kebab case form with at least two words (i.e:some-tag-name)
app.component("friend-contact", FriendContact);
app.mount("#app");
```

In the **App.vue**

```html
<template>
  <friend-contact></friend-contact> // will import the FriendContact component
</template>
<script></script>
```

### Importing a component locally

In the **App.vue**

```html
<template>
  <friend-contact></friend-contact> // will import the FriendContact component
  // or with a self-closing tag <FriendContact /> // will also import the
  FriendContact component,
</template>
<script>
  import FriendContact from './components/FriendContact.vue';

  export default{
    component: {
      'friend-contact': FriendContact
      // or
      FriendContact
    }
  }
</script>
```

### Props

Passing data to a child component can be achieved using **props**.
!Important => a Prop value shouldn't be mutated(changed) inside the child component.

- Using Props
- Supported Props values

#### Using Props

Setting the data in the parent Vue file:

```html
<template>
  <friend-contact
    person-name="someNameProperty"
    person-details="someDetailsProperty"
  ></friend-contact>
</template>
<script>
  export default {
    data() {
      return {
        name: "someNameProperty",
        details: "someDetailsProperty",
      };
    },
  };
</script>
```

Retrieving the data in the child component:

```html
<template>
  <p>{{personName}} - {{personDetails}}</p>
</template>
<script>
  export default{
    // array syntax :
    props:[personName, personDetails] // the kebab-case props will be refactored into camelCase properties by Vue
    // object syntax :
    props: {
      personName: String,
      personDetails: String
    }
    // object syntax with more details :
    props: {
      personName: {
        type: String,
        required: true
      }
      personDetails:{
        type: String,
        required: false,
        default : 0, // can be a value or a function
        default : () => {},
        validator: (valueOfTheProp) => { // will return true/false depending on the condition
          return valueOfTheProp === 'whatever' // will return false unless the value is strictly equal to the string 'whatever'
        }
      }
    }
  }
</script>
```

#### Supported Prop values

- String
- Number
- Boolean
- Array
- Object
- Date
- Function
- Symbol

### Custom Events

Events can be passed from the child component to the parent component.

Parent component:

```html
<template>
  <friend-contact @event-from-child="methodToRun"></friend-contact>
</template>
<script>
  export default {
    methods: {
      methodToRun(dataPassedFromTheChild) {
        // do something with the data
      },
    },
  };
</script>
```

Child component:

The object **$emit** will be used to emit some values. It accepts as many arguments as we wish, but the first one will be the name of the event that will be used in the template of the parent component (@nameOfTheEvent="..."). The other arguments will be the data passed in the event.
The propery **emit** will list all the events emitted by the child component. It is not required but will help in the development process as it can show warnings in the console just like the **props** property does.

```html
<template>
  <p>{{personName}} - {{personDetails}}</p>
</template>
<script>
  export default{
    // array syntax
    emits: ['event-from-child', ...],
    // object syntax
    emits: {
      'event-from-child': (parameterThatWillBePassed)=> {
        if(parameterThatWillBePassed){
          return true
        }else {
          console.warn('parameterThatWillBePassed is missing')
          return false
        }
      }
    }
    methods: {
      methodToEmitSomeEvent(){
        this.$emit('event-from-child', valueToEmit)
      }
    }
  }
</script>
```

### Provide/Inject

**Provide** and **Inject** make it easier to pass data through multiple levels of components instead of using **props**.

Ancestor component:

```html
<template>
  <second-component>Nothing needs to be passed here</second-component>
</template>
<script>
  export default {
    // array syntax
    provide: [dataProvided: 'can be any value'],
    // object syntax to pass properties from the component
    provide(){
      return {
        emitEvent: this.doSomething
      }
    }
    methods: {
      doSomething(value){
        // do something with the value
      }
    }
  };
</script>
```

Second component:

```html
<template>
  <final-component>Nothing needs to be passed here</final-component>
</template>
<script>
  export default {};
</script>
```

Final component. This component can retrive the data of the ancestor component using **Inject**:

```html
<template>
  <p>Final Component : {{this.dataProvided}}</p>
  // will display 'can be any value'
  <button @click="emitEvent(someValue)">Emit an event</button> // 'emitEvent'
  will emit 'someValue'
</template>

<script>
  export default {
    inject: ["dataProvided", "emitEvent"], // this data is provided on the ancestor component
  };
</script>
```

## Asynchronous Components

Components can be loaded asynchronously, so only loaded when we need them, not in the initial bundle, making the initial app load faster. This method can be used on components and routing :

In the main.js file :

```javascript
import { defineAsyncComponent } from "vue";

const BaseDialog = defineAsyncComponent(() =>
  import("./components/ui/BaseDialog.vue")
);

const app = createApp(App);

app.component("base-dialog", BaseDialog); // this component will be loaded only when required
```

In the router file :

```javascript
const SomeComponentName = () =>
  import("./pages/components/SomeComponentName.vue");

const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: "/", redirect: "/coaches" },
    { path: "/coaches", component: SomeComponentName }, // this comopent will be loaded when the route is requested
  ],
});
```

## Composition API

The standard way of using Vuejs is called the **Options API**.
The **Composition API** replaces the **Options API** and makes it easier to manage the data in a component.

- setup
- ref
- computed
- watch
- life cycles

### setup()

The **setup(**) method replaces the "options" **data(), methods, computed, watch**:

### ref

Use the **ref** method to make some properties reactive.

```javascript
<script>
import { ref } from "vue"; // import the ref method

setup(){
  const someConst = ref('someValue')
  function someFunction(){}
  return{
    someConst,
    someFunction
  }
}

</script>
```

### computed

Use the **computed** method to update properties in the template.

```html
<template>
  <input @input="onUpdateFirstName" />
  <input @input="onUpdateLastName" />
  <!-- OR -->
  <input v-model="firstName" />
  <input v-model="lastName" />

  {{ concatName }}
</template>
<script>
  import { ref, computed } from "vue";

  export default {
    setup() {
      const firstName = ref("");
      const lastName = ref("");
      const concatName = computed(
        () => `${firstName.value} - ${lastName.value}`
      );

      function onUpdateFirstName(event) {
        console.log("event", event.target.value);
        firstName.value = event.target.value;
      }
      function onUpdateLastName(event) {
        lastName.value = event.target.value;
      }

      return {
        firstName,
        lastName,
        onUpdateFirstName,
        onUpdateLastName,
        concatName,
      };
    },
  };
</script>
```

### watch

Use the **watch** method to watch the properties changes.

```html
<script>
  import { ref, computed, watch } from "vue";

  export default {
    setup() {
      const firstName = ref("");
      const lastName = ref("");
      const concatName = computed(
        () => `${firstName.value} - ${lastName.value}`
      );

      function onUpdateFirstName(event) {
        console.log("event", event.target.value);
        firstName.value = event.target.value;
      }
      function onUpdateLastName(event) {
        lastName.value = event.target.value;
      }

      watch(lastName, (newValue, oldValue) => {
        console.log(oldValue, newValue);
      });

      return {
        firstName,
        lastName,
        onUpdateFirstName,
        onUpdateLastName,
        concatName,
      };
    },
  };
</script>
```

### life cycles

Vue provides functions for the different life cycles:

- onBeforeMount
- onMounted
- onBeforeUpdate
- onUpdated
- onBeforeUnmount
- onUnmounted

=> There is an exeption for **createApp** and **beforeCreate** as these two are basically called inside the **setup()** method. So any code that would run inside them can be called in the **setup()** method.

```html
<script>
  import {onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted } from 'vue';

  export default {
    setup() {

      onBeforeMount(() => console.log('onBeforeMount'))
      onMounted(() => console.log('onMounted'))
      ...
    },
  };
</script>
```

## Scoped styles

Styles defined in the **style** tag of a component will affect the **entire** application by default:

```html
<style>
  header {
    background: blue;
    color: red;
    height: 50px;
  }
</style>
```

By adding the **scoped** attribute the style will only affect the component it is attached to:

```html
<style scoped>
  header {
    background: blue;
    color: red;
    height: 50px;
  }
</style>
```

## Slots

- Single slot
- Named slots
- Default content
- Conditional display
- Scoped slots
- Dynamic components
- Keep Alive
- Teleport

**Slots** are like Props but for HTML content. They will wrap some HTML and display it inside the **slot** tags.

### Single slot

In the **reusable** component:

```html
<template>
  <div>
    <slot></slot>
  </div>
</template>

<style scoped>
  div {
    margin: 2rem auto;
    border: 1px solid red;
  }
</style>
```

In some other component:

```html
<template>
  <component-name>
    <header>
      <h1>Some content</h1>
      <p>Some description...</p>
    </header>
  </component-name>
</template>
```

### Named slot

Multiple **slots** can be uesd by _naming_ them. Only one slot can be used as **default**, all others must be named:

```html
<template>
  <header>
    <slot name="header"></slot> // here the slot has a name attribute with the
    header parameter
  </header>
  <section>
    <slot name="default"></slot>
  </section>
</template>
```

In some other component, use the **v-slot:NameOfTheslot** (or **#NameOfTheSlot**) attribute :

```html
<template>
  <component-name>
    <template v-slot:header>
      // here the template tag will display the header slot
      <h1>Some content</h1>
      <p>Some description...</p>
    </template>
    <template #default>
      // here the default slot will be used. Same as v-slot:default
      <p>Some content...</p>
    </template>
  </component-name>
</template>
```

### Default content

Slots can have default content that will be displayed in all the components that use them:

```html
<template>
  <header>
    <slot name="header">
      <p>This is some default content</p>
      // here the paragraph will be displayed as default content
    </slot>
  </header>
  <section>
    <slot name="default"></slot>
  </section>
</template>
```

### Conditional display

If a component doesn't use all the slots available, use v-if="$slots.NameOfTheSlot" to avoid loading an empty tag:

```html
<template>
  <header v-if="$slots.header">
    // here Vue will filter the components using the slot named "header"
    <slot name="header">
      <p>This is some default content</p>
    </slot>
  </header>
  <section>
    <slot name="default"></slot>
  </section>
</template>
```

### Scoped slots

Passing Props to a slot:

Reusable component:

```html
<template>
  <section>
    <ul>
      <li v-for="goal of list" :key="item">
        <slot
          :item="goal"
          another-prop="can be anything"
          name="slotList"
        ></slot>
      </li>
    </ul>
  </section>
</template>

<script>
  export default {
    data() {
      return {
        list: ["some item", "some other item", "yet another item"],
      };
    },
  };
</script>
```

Other component:

```html
<template>
  <reusable-component>
    <template #slotList="slotProps">
      // will load the props
      <p>{{ slotProps.goal}}</p>
      // will display the goal
      <p>{{ slotProps['another-prop']}}</p>
      // will the display 'can be anything'
    </template>
  </reusable-component>
  // or
  <reusable-component #slotList="slotProps">
    // will load the props
    <p>{{ slotProps.goal}}</p>
    // will display the goal
    <p>{{ slotProps['another-prop']}}</p>
    // will the display 'can be anything'
  </reusable-component>
</template>
```

### Dynamic components

Components can be loaded using the **component** tag:

```html
<template>
  <button @click="setSelectedComponent('component-one')">Select One</button>
  <button @click="setSelectedComponent('component-two')">Select Two</button>
  <component :is="selectedComponent"></component>
</template>

<script>
  import ComponentOne from './ComponentOne.vue';
  import ComponentTwo from './ComponentTwo.vue';
    data(){
      return{
        selectedComponent: 'component-one' // will be the default component loaded
      }
    },
    methods: {
      setSelectedComponent(cmp){
        return this.selectedComponent = cmp
      }
    }
</script>
```

### Keep Alive

The **keep-alive** tag will keep the component in the cache, so the component wil not be _destroyed_, it will not be removed from the DOM :

```html
<template>
  <button @click="setSelectedComponent('component-one')">Select One</button>
  <button @click="setSelectedComponent('component-two')">Select Two</button>
  <keep-alive>
    // this will make sure the component keeps its data alive
    <component :is="selectedComponent"></component>
  </keep-alive>
</template>
```

### Teleport

The **teleport** tag allows any content to be placed at the desired position in the HTML tree using the **to** attribute. It can be any CSS selector:

- body
- #app
- .some-css-classes
- ...

```html
<template>
  <teleport to="body">
    // now the dialog tag will be placed at the root of the HTML tree
    <dialog open>
      <slot></slot>
    </dialog>
  </teleport>
</template>
```

## Forms

- Basic form-controls
- Custom form-controls

```html
<template>
  <form @submit.prevent="submitForm">
    // will submit the form without reloading the page
    <div class="form-control">
      <label for="user-name">Your Name</label>
      <input
        id="user-name"
        name="user-name"
        type="text"
        v-model.trim="userName"
      />
      // the value will be trimed by default
    </div>
    <div class="form-control">
      <label for="age">Your Age (Years)</label>
      <input id="age" name="age" type="number" v-model.number="userAge" /> //
      the value will be casted as a number
    </div>
    <div class="form-control">
      <label for="favoriteAvenger">Who is your favorite Avenger?</label>
      <select
        id="favoriteAvenger"
        name="favoriteAvenger"
        v-model="favoriteAvenger"
      >
        <option value="iron">Iron Man</option>
        <option value="thor">Thor</option>
        <option value="captain">Captain America</option>
      </select>
    </div>
    <div class="form-control">
      <h2>Who would you choose for your team ?</h2>
      <div>
        <input
          id="thor"
          name="team"
          type="checkbox"
          value="thor"
          v-model="dreamTeam"
        />
        <label for="thor">Thor</label>
      </div>
      <div>
        <input
          id="ironman"
          name="team"
          type="checkbox"
          value="ironman"
          v-model="dreamTeam"
        />
        <label for="ironman">Iron Man</label>
      </div>
    </div>
    <div class="form-control">
      <h2>Favorite power?</h2>
      <div>
        <input
          id="flight"
          name="power"
          type="radio"
          value="flight"
          v-model="power"
        />
        <label for="flight">Video Courses</label>
      </div>
      <div>
        <input
          id="wallcrawling"
          name="power"
          type="radio"
          value="wallcrawling"
          v-model="power"
        />
        <label for="wallcrawling">Blogs</label>
      </div>
    </div>
    <div class="form-control">
      <h2>Agree to terms ?</h2>
      <div>
        <input id="terms" name="how" type="checkbox" v-model="terms" />
      </div>
    </div>
    <div>
      <button>Save Data</button>
    </div>
  </form>
</template>

<script>
  export default {
    data() {
      return {
        userName: "",
        userAge: "",
        dreamTeam: [],
        power: [],
        terms: true,
      };
    },
    methods: {
      submitForm() {
        // do stuff here...
      },
    },
  };
</script>
```

### Custom form-controls

Parent component:

```html
<template>
  <customp-form-control v-model="nameOfEvent"></customp-form-control>
  // same as :
  <customp-form-control
    :model-value=""
    @update:modelValue=""
  ></customp-form-control>
</template>
```

Child component:

```html
<template> </template>
<script>
  export default {
    props: ["modelValue"],
    emits: ["update:modelValue"],
  };
</script>
```

## Routing

- [Install](#install)
- [Creating the routes](#creating-the-routes)
- [Loading the views](#loading-the-views)
- [Navigating (template)](#navigating-template)
- [Navigating (programmatically)](#navigating-programmatically)
- [Styling the active route](#styling-the-active-route)
- [Route Params (URL)](#route-params-url)
- [Route Params (Props)](#route-params-props)
- [Redirecting](#redirecting)
- [Page Not Found](#page-not-found)
- [Nested Routes](#nested-routes)
- [Named Routes](#named-routes)
- [Query Params](#query-params)
- [Named Router Views](#named-router-views)
- [Scroll Behavior](#scroll-behavior)
- [Navigation Guards](#navigation-guards)

### Install

Install a routing package such as **vue-router**:
`npm i --save vue-router@next`

### Creating the routes

Import the **createRouter** method and define the **router** variable that will hold the different routes in the _main.js_ file:

```javascript
import { createApp } from "vue";
import { createRouter, createWebHistory } from "vue-router";

import ComponentOne from "./components/ComponentOne.vue";
import ComponentTwo from "./components/ComponentTwo.vue";

import App from "./App.vue";

const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: "/component-one", component: ComponentOne },
    { path: "/component-two", component: ComponentTwo },
  ],
});

const app = createApp(App);

app.use(router);

app.mount("#app");
```

### Loading the views

Import the **router-view** tag in the desired component. It will act as a placeholder for the components to load:

```html
<template>
  <the-navigation></the-navigation>
  <main><router-view></router-view> // the components will be loaded here</main>
</template>
```

### Navigating (template)

To navigate, use the **router-link** tags:

```html
<template>
  <header>
    <nav>
      <ul>
        <li>
          <router-link to="/component-one">Component One</router-link>
        </li>
        <li>
          <router-link to="/component-two">Component Two</router-link>
        </li>
      </ul>
    </nav>
  </header>
</template>
```

### Navigating (programmatically)

Use the **this.$router** method:

```html
<template>
  <button @click="navigate">Navigate</button>
</template>

<script>
  export default{
    methods: {
      navigate() {
        this.$router.push('/pathToComponent')
        this.$router.replace('/pathToComponent') // will replace the current location
        this.$router.back()
        this.$router.forward()
        ...
      }
    }
  }
</script>
```

### Styling the active route

The router adds two classes to the active link:

- router-link-active
- router-link-exact-active

```html
<style>
  a.router-link-active {
    /* Style goes here */
  }
  a.router-link-exact-active {
    /* Style goes here */
  }
</style>
```

### Route Params (URL)

Use the **:paramName** in the path to pass parameters to the route:

```javascript
const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: "/teams", component: TeamsList },
    { path: "/teams/:paramName", component: ComponentToLoad },
  ],
});
```

Retrieve the params in the component using the **$route** object:

```html
<script>
  export default {
    methods: {
      loadMembers(route) {
        const teamId = route.params.paramName;
      },
    },
  };
</script>
```

### Route Params (Props)

Add the **props** property to pass the param as a Prop:

```javascript
const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: "/teams", component: TeamsList },
    {
      path: "/teams/:paramName",
      component: ComponentToLoad,
      props: true, // the param will be passed in as a Prop
    },
  ],
});
```

Retrieve the params via the **Props** property:

```html
<script>
  export default {
    props: ["paramName"],
  };
</script>
```

### Redirecting

Add the **redirect** property:

```javascript
const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: "/", redirect: "/somePath" },
    { path: "/teams", component: TeamsList },
  ],
});
```

### Page not found

```javascript
const router = createRouter({
  routes: [
    { path: "/", redirect: "/somePath" },
    { path: "/teams", component: TeamsList },
    { path: "/:anyName(.*)", component: PageNotFound },
    // or
    { path: "/:anyName(.*)", redirect: "/somePath" },
  ],
});
```

### Nested Routes

Use the **children** property to nest a route:

```javascript
const router = createRouter({
  routes: [
    { path: "/", redirect: "/somePath" },
    {
      path: "/teams",
      component: TeamsList,
      children: [{ path: ":teamId", component: TeamMembers, props: true }],
    },
  ],
});
```

In the parent component add another **router-link** tag:

```html
<template>
  <router-link></router-link> // the children will be loaded here
</template>
```

### Named Routes

Add the **name** property:

```javascript
const router = createRouter({
  routes: [
    { name: "root", path: "/", redirect: "/somePath" },
    {
      name: "teams",
      path: "/teams",
      component: TeamsList,
      children: [{ path: ":teamId", component: TeamMembers, props: true }],
    },
  ],
});
```

Then in any component:

```html
<template>
  <router-link :to="someProperty">Navigate</router-link>
</template>
<script>
  export default {
    computed: {
      someProperty() {
        return { name: "nameOfThePath", params: { nameOfTheParam: value } };
      },
    },
  };
</script>
```

### Query Params

Passing data via query params using the **query** property:

```javascript
const router = createRouter({
  routes: [
    { name: "root", path: "/", redirect: "/somePath" },
    {
      path: "/teams",
      component: TeamsList,
      query: { someKey: "someValue" }, // will output "http://.../teams?somekey=someValue" in the URL
    },
  ],
});
```

Retrieving the query in the component:

```html
<script>
  export default {
    created() {
      console.log(this.$route.query); // will output the query params
    },
  };
</script>
```

### Named Router Views

The **router-view** tag can have a _name_ attribute. So different components can be loaded depending on the name:

```javascript
const router = createRouter({
  routes: [
    { name: "root", path: "/", redirect: "/somePath" },
    {
      name: "teams",
      path: "/teams",
      components: {
        default: TeamsList,
        footer: TeamFooter,
      },
    },
    {
      path: "/users",
      components: {
        default: UsersList,
        footer: UserFooter,
      },
    },
  ],
});
```

```html
<template>
  <main>
    <router-view></router-view> // will act as the "default" router, it doesn't
    need to be named
  </main>
  <footer>
    <router-view name="footer"></router-view> // will act as the "named" router
  </footer>
</template>
```

### Scroll Behavior

Add the **scrollBehavior** property to the route variable :

```javascript
const router = createRouter({
  routes: [{ name: "root", path: "/", redirect: "/somePath" }],
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition; // will return the previous scroll position
    }
    return { left: 0, top: 0 }; // will set the page to the top on route change
  },
});
```

### Navigation Guards

Navigation guards are used to prevent the access of certain routes. Add a **meta** field to the routes item with some custom property to check when navigating.
Then use the **beforeEach** method to check the meta property.

```javascript
import { createRouter, createWebHistory } from "vue-router";

import CoachDetail from "./pages/coaches/CoachDetail.vue";
import CoachesList from "./pages/coaches/CoachesList.vue";
import CoachRegistation from "./pages/coaches/CoachRegistration.vue";
import ContactCoach from "./pages/requests/ContactCoach.vue";
import RequestsReceived from "./pages/requests/RequestsReceived.vue";
import UserAuth from "./pages/auth/UserAuth.vue";
import NotFound from "./pages/NotFound.vue";
import store from "./store/index.js";

const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: "/", redirect: "/coaches" },
    { path: "/coaches", component: CoachesList },
    {
      path: "/coaches/:id",
      component: CoachDetail,
      props: true,
      children: [
        { path: "contact", component: ContactCoach }, // /coaches/c1/contact
      ],
    },
    {
      path: "/register",
      component: CoachRegistation,
      meta: { requiresAuth: true },
    }, // this route requires to be authenticated
    {
      path: "/requests",
      component: RequestsReceived,
      meta: { requiresAuth: true },
    }, // this route requires to be authenticated
    { path: "/auth", component: UserAuth, meta: { requiresUnauth: true } }, // this route requires to be unauthenticated
    { path: "/:notFound(.*)", component: NotFound },
  ],
});

router.beforeEach((to, from, next) => {
  if (to.meta.requiresAuth && !store.getters.loggedIn) {
    next("/auth");
  } else if (to.meta.requiresUnauth && store.getters.loggedIn) {
    next("/coaches");
  } else {
    next();
  }
});

export default router;
```

## Animations

- Transition component
- Custom CSS Transition class
- Transitioning between multiple elements
- Transition events
- Disabling CSS Transitions
- Animating lists
- [Animating Route changes](#animating-route-changes)

### Transition component

Vue has a **transition** component that can be added as a wrapper to any DOM element. It needs to be wrapped around one element only(with some exceptions, see #Transitioning between multiple elements) :

```html
<template>
  <div>
    <transition>
      <div v-if="displayDiv">Div to toggle...</div>
    </transition>
    <button @click="displayDiv = !displayDiv">Toggle</button>
  </div>
</template>
```

It will add special classes to the tag:

- v-enter-from
- v-enter-to
- v-enter-active
- v-leave-from
- v-leave-to
- v-leave-active

These classes can then be used in the **style** tag:

```javascript
<style>
  .v-enter-from{}
  .v-enter-to{}
  .v-enter-active{}
  .v-leave-from{}
  .v-leave-to{}
  .v-leave-active{}
</style>
```

### Custom CSS Transition class

The special classes can be named by adding attributes to the transition tag.
Available attributes :

- name
- enter-to-class
- enter-from-class
- enter-active-class
- leave-to-class
- leave-from-class
- leave-active-class

```html
<template>
  <div>
    <transition name="any-name">
      // here the name attribute is added
      <div v-if="diplayDiv">Div to toggle...</div>
    </transition>
    <button @click="displayDiv = !displayDiv">Toggle</button>
  </div>
</template>

// the .v will then be changed by .any-name

<style>
  .any-name-enter-from {
  }
  .any-name-enter-to {
  }
  .any-name-enter-active {
  }
  .any-name-leave-from {
  }
  .any-name-leave-to {
  }
  .any-name-leave-active {
  }
</style>
```

### Transitioning between multiple elements

The transition component can be wrapped around multiple elements as long as we ensure that only **one** element will be visible at a time :

```html
<template>
  <div>
    <transition mode="out-in">
      <button @click="hide" v-if="isDisplayed">Hide</button>
      <button @click="show" v-else>Show</button>
    </transition>
  </div>
</template>
```

Then use the **mode** attribute to make the animation look better :

- in-out => will first animate the element that enters
- out-in => will first animate the element that leaves

### Transition events

Vue has **transition events** that can be listened to :

- @before-enter
- @enter
- @after-enter
- @before-leave
- @leave
- @after-leave
- @enter-cancelled
- @leave-cancelled

```html
<template>
  <div>
    <transition
      @before-enter="method"
      @enter="method"
      @after-enter="method"
      @before-leave="method"
      @leave="method"
      @after-leave="method"
      @enter-cancelled="method"
      @leave-cancelled="method"
      mode="out-in"
    >
      <button @click="hide" v-if="isDisplayed">Hide</button>
      <button @click="show" v-else>Show</button>
    </transition>
  </div>
</template>
```

### Disabling CSS Transitions

The CSS classes can be disabled if the animations are only controled with Javascript, this can improve performance as Vue won't parse the CSS files :

Add `:css="false"` to the transition tag.

```html
<template>
  <div>
    <transition :css="false" @before-enter="method" mode="out-in">
      <button @click="hide" v-if="isDisplayed">Hide</button>
      <button @click="show" v-else>Show</button>
    </transition>
  </div>
</template>
```

### Animating lists

Use the **transition-group** tag to animate lists. It will replace the **ul** tag :

```html
<template>
  <div>
    <transition-group tag="ul" name="list">
      <li v-for="item in list">{{ item }}</li>
    </transition-group>
  </div>
</template>

<style>
  .list-enter-from {
    opacity: 0;
    transform: translateX(-30px);
  }
  .list-enter-to {
    opacity: 0;
    transform: translateX(0);
  }
  .list-move {
    // (.v-move)
    transition: transform 0.8s ease; // will animate the other elements
  }
</style>
```

### Animating Route changes

Route changes can be animated using the transition component inside the **router-view** tag.
_!IMPORTANT => the route components must have only **one** root element_.

```html
<template>
  <router-view v-slot="slotProps">
    <transition name="route" mode="out-in">
      <component :is="slotProps.Component"></component>
    </transition>
  </router-view>
</template>

<style>
  .route-enter-active {
    animation: slide-fade 0.3s ease-in;
  }
  .route-leave-active {
    animation: slide-fade 0.3s ease-out reverse;
  }

  @keyframes slide-fade {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }
</style>
```

## Vuex

**Vuex** helps managing the state/data of the application by outsourcing said state/data and making them available application wide.

- Install
- Create a store
- $store
- Mutations
- Payload
- Getters
- Actions
- Mapper Helpers
- Modules
- Namespacing Modules

### Install

Install via npm:
`npm i --save vuex@next`

### Create a store

Once a store is created, the values will be available in every components.

```javascript
import { createApp } from "vue";
import { createStore } from "vuex";

import { App } from "./App.vue";

const app = createApp(App);

const store = createStore({
  state: {
    someProperty: value,
    somePropertyTwo: value,
  },
});
```

### $store

Accessing the data in a component via **$store**:

```html
<template>
  <p>{{ someName }}</p>
</template>

<script>
  export default {
    computed: {
      someName() {
        return this.$store.state.someProperty;
      },
    },
  };
</script>
```

### Mutations

**Mutations** are like functions that will manage the state/data and will allow the components to make changes on those state/data but without directly changing their value.
They **shouldn't** have any asynchronous code inside them, for that use **actions** instead.

```javascript
import { createApp } from 'vue';
import { createStore } from 'vuex';

import { App } from './App.vue';

const app = createApp(App);

const store = createStore({
  state: {
    someProperty: value,
    somePropertyTwo: value,
  },
  mutations: {
    someMethod(state) {
      // all mutations will have the state object available
      state.counter++;
    },
    someOtherMethodsWithPayload()
  },
});
```

In any component, use the **$commit** method to call a method from the mutations object :

```html
<template>
  <p>{{ counter }}</p>
  <button @click="addOne">Add</button>
</template>

<script>
  export default {
    computed: {
      counter() {
        return this.$store.state.counter;
      },
    },
    methods: {
      addOne() {
        this.$store.commit("someMethod"); // the string should be equal to the method name in the mutations object
      },
    },
  };
</script>
```

### Payload

**Payload** is another parameter passed to a mutation method that will hold some value passed from a component:

```javascript
const store = createStore({
  state: {
    someProperty: value,
    somePropertyTwo: value,
  },
  mutations: {
    someMethodWithPayload(state, payload) {
      state.counter += payload.value;
    },
  },
});
```

```html
<script>
  export default {
    methods: {
      increase() {
        this.$store.commit("someMethodWithPayload", { value: 10 }); // 10 here will be passed as an argument
        // or the object syntax
        this.$store.commit({
          type: "someMethodWithPayload",
          value: 10,
        }); // 10 here will be passed as an argument
      },
    },
  };
</script>
```

### Getters

The **state** shouldn"t be accessed directly, instead **Getters** should be used.
They are used to modify the state/data and then making them available to any components:

```javascript
const store = createStore({
  state: {
    someProperty: value,
    somePropertyTwo: value,
  },
  getters: {
    someMethod(state) {
      return state.somePropertyTwo;
    },
  },
});
```

```html
<script>
  export default {
    computed: {
      getStore() {
        return this.$store.getters.someMethod;
      },
    },
  };
</script>
```

### Actions

**Actions** are used to run asynchronous code.

```javascript
const mainStore = createStore({
  mutations: {
    login(state) {
      return (state.loggedIn = true);
    },
  },
  actions: {
    login(context) {
      console.log("login actions", context);
      return context.commit("login");
    },
  },
});
```

### Mapper Helpers

- mapGetters
- mapActions

#### mapGetters

```html
<script>
  import { mapGetters } from "vuex";

  export default {
    computed: {
      ...mapGetters(["nameOfTheGetter"]),
    },
  };
</script>
```

- mapActions

#### mapActions

```html
<template>
  <button @click="nameOfTheAction({value: someValue})">Click</button>
</template>

<script>
  import { mapActions } from "vuex";

  export default {
    methods: {
      ...mapActions(["nameOfTheAction"]),
    },
  };
</script>
```

### Modules

Create **store modules** to split the code:

```javascript
const someModule = {
    namespace = true,
    state() {
        return {
            counter: 0,
        }
    },
    mutations: {
        someMethod(state) {
            // all mutations will have the state object available
            state.counter++
        },
    },
    getters: {
        increasedByTen(state) {
            return state.counter + 10
        },
    },
    actions: {
        increase(context, payload) {
            return context.commit('increase', payload)
        },
    }
}

const mainStore = createStore({
    modules: {
        counter: someModule
    },
    ...
```

### Namespacing Modules

Namespacing modules will make sure there are no clash in naming regarding the actions, getters and actions.
