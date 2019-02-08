# Using Forms in Vue.js

In Vue.js we can use forms to gather data from the user, and the form fields and events can easily be connected to our component logic. To accomplish this, we create bindings between the form fields and our component data. The `v-model` directive is how we assign these relationships. By binding fields to data values, we can do a lot directly in templates, or we can create event listeners and use component methods to perform actions when the form is submitted or when another event occurs.

## Basic Input Data Binding with `v-model`
The `v-model` directive associates a form input element with a variable in our component's data. This variable can be referenced within the component logic (in computed values, methods, or filters), or it can be referenced within the template for display to the user. The `v-model` directive creates a two-way binding between the value in the component logic and the value in the template, just like other variables we define in the component's `data()` function.

We can create an association like the one in this example component:

```html
<template>
  <div class="forms">
    <h2>{{ message }}</h2>
    <label>Message: <input type="text" v-model="message"></label>
    <p>Computed reversed message: {{ reversedMessage }}</p>
  </div>
</template>

<script>
export default {
  name: 'FormsPractice',
  data () {
    return {
      message: 'Type in the input to change the message.'
    }
  },
  computed: {
    reversedMessage: function () {
      return this.message.split('').reverse().join('')
    }
  }
}
</script>
```
We can see here that we have a simple Vue.js component called `FormsPractice`. This component uses a data object that includes the `message` property. The `message` property is used in the template, where it is interpolated between the `<h1>` tags. The `message` property is defined in the component logic as part of the object returned by the `data()` function. There is also a computed value defined called `reversedMessage`, which also uses the `message` value. 

All of these values are properly bound so that when the content of the text field is altered those changes are reflected in the HTML so the user can see them. Here is an example:

![Animation of form field model binding](/img/form-model-bind1.gif)
<br>Animation of form field model binding

Since the default binding is updated whenever the form field is altered, we see immediate reaction to the user's input in the HTML. (If immediate reflection of the changed value is not desirable, we can use the `.lazy` modifier below to update the bound values only on the `change` event instead of the `input` event. See below for further explanation.)

## Generating Options and Binding Groups of Inputs
There are a few kinds of form field inputs that rely upon or provide grouped information. Options for selects, checkbox groups, and radio buttons are often generated from an Array of data. Multiple selects and checkbox groups can also return an array of data for processing. Luckily, these input fields are well-connected in the Vue.js framework, and we can work with their data like any other data in the system.

We can use the `v-bind` directive to populate a form field with saved data. In many of the examples below, we use the `v-bind` directive to attach values to `<option>` and checkbox fields that are generated by looping through a data Array. As with any other HTML element attribute, we can use `v-bind` to create a binding between some value or computation and the attribute itself. This is a common case that we encounter regularly in developing forms for our applications, so it's worth noting that we can easily use `v-bind` with the `value` attribute.

Here is an example of a simple `<select>` field setup:

```html
<template>
  <div class="forms">
    <h2>Selected: {{ petSelection }}</h2>
    <label for="petChooser">Pick a pet:</label>
    <select v-model="petSelection">
      <option disabled value="">Please select one</option>
      <option v-for="pet in petList" v-bind:value="pet">{{ pet }}</option>
    </select>
  </div>
</template>

<script>
export default {
  name: 'FormsPractice',
  data () {
    return {
      petList: [
        'dog',
        'cat',
        'pig'
      ],
      petSelection: 'pig'
    }
  }
}
</script>
```
In this example, we have a component that defines a `petList` Array and a String called `petSelection`. The template displays the current selection as text, and the `<select>` element is bound to the `petSelection` value. The `<option>` elements are generated using a `v-for` loop. (Note that the `v-bind:value` directive can access the item in each iteration of the loop.) The result is an interface that works like this:

![Bound select input](/img/form-model-bind2-select.gif)
<br>Bound select input

The selection list is automatically initialized with the current value of the `pet` value specified in the `v-model` attribute. When the value is updated, the output is correspondingly updated. Similarly, we can look at an example of a checkbox group to get an idea of how that works.

```html
<template>
  <div class="forms">
    <h2>Selected: {{ petSelection }}</h2>
    <label v-for="pet in petList">
      {{ pet }}
      <input type="checkbox" v-model="petSelection" v-bind:value="pet">
    </label>

  </div>
</template>

<script>
export default {
  name: 'FormsPractice',
  data () {
    return {
      petList: [
        'dog',
        'cat',
        'pig'
      ],
      petSelection: ['pig']
    }
  }
}
</script>
```
In this slightly modified version of the previous example, we have altered the `petSelection` value to be an Array. We have initialized its value with one item: `['pig']`. In the template, rather than looping the `<input>` tag, we have applied the `v-for` loop to the `<label>` tag so we can get proper wrapping of our checkbox inputs. The `v-model` attribute for each checkbox input is set to the same value, so all the values the user clicks will be added to the `petSelection` Array. Finally, the value of each item in the `petList` Array is bound to the `value` attribute using the `v-bind:value="pet"` directive.

Here is what it looks like in action:

![Checkbox group example](/img/form-model-bind3-checkboxgroup.gif)
<br>Checkbox group example

Notice that the value the `petSelection` Array was initialized with is pre-checked when the input is displayed to the user. The Vue.js binding system keeps the checkboxes updated according to the value of the context variable each input is bound to in the template.

<div class="tip-box">
  <h3>Include a Dummy Choice!</h3>
  <p>Due to the way some devices handle events on `<select>` elements, it's considered best practice to include a "dummy choice" at the beginning of any list of selectable items. Note in the example above that we begin the list with a single, disabled option that instructs the user to "select a pet". The dummy choice will be selected by the browser as the default selection shown when the form loads. A user who encounters the form for the first time will have to select another choice, since the dummy choice is disabled.</p> 
  <p>This technique insures that we will always receive a data update when the user makes a selection. Without the dummy choice, it's possible that the user would want to select the first item in the list, and the device or browser might not always trigger a data sync throughout our Vue.js application.</p>
</div>

## Model Modifiers
Sometimes we need to modify the data, or how we collect the data, in a predictable way. Vue.js provides three modifiers we can use with the `v-model` directive to change the way data is bound. These allow us to provide a better user experience in many situations. 

### `.lazy`
The `.lazy` modifier allows us to alter the way data is updated throughout the system upon user input. Normally, a bound input field will update data on the `input` event fired by the field. This can be beneficial, but sometimes it is not desirable. When we would prefer to fire the event on the `change` event (as opposed to the `input` event), we can use the `.lazy` modifier. 

Here is what this would look like on a text input:

```html
<input type="text" v-model.lazy="username">
```

### `.number`
The `.number` modifier insures the value is typecast as a Number when it enters the component logic. By default, any information coming out of a text input field will be typed as a String in our JavaScript logic. If we use the `.number` modifier, we can be sure our calculations will work as planned. Here is an example:

```html
<input type="number" v-model.number="myNumber">
```

Although this example uses the input type `number`, **all** HTML inputs are typed as Strings when they are brought into the Vue.js component logic. If we wish for a value to be cast as a Number, we must use the `.number` modifier.

### `.trim`
We can use the `.trim` modifier to remove extra whitespace from the beginning and end of a user's data. This is often used on login forms when we wish to discount any leading or trailing spaces that might result from a user's copy/paste. We can apply this modifier just like the others:

```html
<input type="text" v-model.trim="username">
```


