# Setup & Reactive References

At this point, you’re probably wondering what the new composition API syntax looks like, and we’re about to dive in. First, we want to be clear about when you might use it, and then we’ll learn about the setup function and Reactive References or refs. Also, you may want to grab Vue Mastery’s [Vue 3 Cheat Sheet](https://www.vuemastery.com/vue-3-cheat-sheet) if you haven’t yet.

**Disclaimer:** If you haven’t caught on yet, the composition API is purely additive, and nothing is deprecated. You can code Vue 3 the same way you code up Vue 2.

## When to use the Composition API?

When any of the following are true:

- You want optimal TypeScript support.
- The component is too large and needs to be organized by feature.
- Need to reuse code across other components.
- You & your team prefer the alternative syntax.

**Disclaimer #2:** The examples that follow are quite simple. Constructing components this simple using the Composition API would be unnecessary, but make it easier to learn.

Let’s start with a very simple component written using our normal Vue 2 API, which is valid in Vue 3.

```html
<template>
  <div>Capacity: {{ capacity }}</div>
</template>
<script>
export default {
  data() {
    return {
      capacity: 3
    };
  }
};
</script>
```

Notice I have a simple `capacity` property, which is reactive. Vue knows to take each of the properties in the object returned by the data property, and make them reactive. This way, when these reactive properties get changed, components that use this property gets re-rendered. Duh, right? In the browser we see:

![img](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1570469941666_01-browser.jpg?alt=media&token=9c8eecfd-cb4a-43b5-98e0-cccd02ffe23d)

## Using the Setup Function

In Vue 3 using the Composition API, I would start by writing that `setup` method we’ve seen before:

```html
<template>
  <div>Capacity: {{ capacity }}</div>
</template>
<script>
export default {
  setup() {
    // more code to write
  }
};
</script>
```

The `setup` executes before any of the following options are evaluated:

- Components
- Props
- Data
- Methods
- Computed Properties
- Lifecycle methods

It’s also worth mentioning that the `setup` method does not have access “this”, unlike other Component options. In order to get access to the properties we normally would access using this, `setup` has two optional arguments. The first is props which is reactive and can be watched, as such:

```javascript
import { watch } from "vue";
export default {
  props: {
    name: String
  },
  setup(props) {
    watch(() => {
      console.log(props.name);
    });
  }
};
```

The second argument is context, that has access to a bunch of useful data:

```javascript
setup(props, context) {
  context.attrs;
  context.slots;
  context.parent;
  context.root;
  context.emit;
}
```

But let’s get back to our example. Our code needs a reactive reference:

## Reactive References

```html
<template>
 <div>Capacity: {{ capacity }}</div>
</template>
<script>
import { ref } from "vue";
export default {
  setup() {
    const capacity = ref(3);
    // additional code to write
  }
};
</script>
```

`const capacity = ref(3)` is creating a “Reactive Reference.” Basically it’s wrapping our primitive integer (3) in an object, which will allow us to track changes. Remember, previously our `data()` option already wrapping our primitive (capacity) inside an object.

***Aside:*** *The composition API allows us to declare reactive primitives that aren’t associated with a component, and this is how we do it.*

One last step, we need to explicitly return an object with properties our template will need to render properly.

```html
<template>
  <div>Capacity: {{ capacity }}</div>
</template>
<script>
import { ref } from "vue";
export default {
  setup() {
    const capacity = ref(3);
    return { capacity };
  }
};
</script>
```

This returned object is how we expose which data we need access to in the `renderContext`.

Being explicit like this is a little more verbose, but it’s also intentional. It helps with longer-term maintainability because we can control what gets exposed to the template, and trace where a template property is defined. We now have what we started with:

![img](https://firebasestorage.googleapis.com/v0/b/vue-mastery.appspot.com/o/flamelink%2Fmedia%2F1570469941666_01-browser.jpg?alt=media&token=9c8eecfd-cb4a-43b5-98e0-cccd02ffe23d)

## Using Vue 3 with Vue 2

It’s worth noting that you can use the Vue 3 Composition API with Vue 2 today by using [this plugin](https://github.com/vuejs/composition-api). Once you install and configure it on a Vue 2 application, you’d use the same syntax I’m teaching you above with one small change. Instead of

```javascript
import { ref } from "vue";
```

you would write

```javascript
import { ref } from "@vue/composition-api";
```

That’s how I tested all the above code, in case you’re wondering.

Next, we’ll learn how to write component methods using this new syntax.