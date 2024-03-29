# Slots: Techniques

## Introduction

Now that you have the fundamental skills to use slots, it’s time to further our understanding of more complex slot techniques.

## What if you need to define multiple slots?

While having the ability to define a single slot is empowering, there may be times where you need a component to have the ability to receive content in multiple slots. To accomplish this, you need to understand two concepts: named slots and `template` blocks.

### What are named slots?

Named slots are a way to configure your slots so that they are unique from one another. To do this, slots have a `name` property that allows you to designate it with a custom identifier.

By default, all slots are actually assigned the name `default`. Using our `BaseButton` example from the last lesson, these two snippets of code are identical.

```html
// Using implicit naming
<template>
  <button>
    <slot></slot>
  </button>
</template>

// Using explicit naming
<template>
  <button>
    <slot name="default"></slot>
  </button>
</template>
```

While not important when using a single slot, the ability to name your slots is powerful when using multiple slots.

Let’s take on an example where you have a BlogLayout component responsible rendering out content in a certain style. However, you need to pass content for the header, main, and footer section into different slots. This could be accomplished with the following snippet:

**📄BlogLayout.vue**

```html
<div class="blog-container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

But the question you may be wondering next is, how would this look when using the BlogLayout component? To accomplish this, we’ll need the `template` element.

### What is the template block?

Similar to how you use the `<template>` block to define the HTML of your single file components, you can also use it to define specific blocks of HTML that need to be passed into a slot. This can be accomplished by applying the `v-slot` directive to the `template` element.

The `v-slot` directive can then take an argument which is the desired slot name you want the content to appear in.

Here is an example of our `BlogLayout` component being used with this new syntax we just learned.

📄**App.vue**

```html
<template>
  <BlogLayout>
    <template v-slot:header>
      <h1>My Blog Title</h1>
    </template>
    <template>
      <p>Main body content</p>
    </template>
    <template v-slot:footer>
      <p>Footer content</p>
    </template>
  </BlogLayout>
</template>
```

## What if I need to programmatically generate slot names?

While things like layouts are often fixed, there are situations we need to programmatically generate our slots. For example, an application may allow users to customize the layout, which is saved on the backend and our Vue.js application renders out what the API returns to us. In this case, we can use dynamic slot names to accomplish this.

The syntax is similar to how you would normally use slot names, except instead of passing the name of the slot, you pass a variable by wrapped in square brackets.

```html
// Normal named slot
<template v-slot:header>...</template>

// Dynamic named slot
<template v-slot:[dynamicSlotNameVariable]>...</template>
```

Using our `BlogLayout` example, here’s how you would programmatically generate the slot names.

📄**App.vue**

```html
<script>
export default {
  data() {
    return {
      layout: [
        { name: 'header', content: 'My Blog Title' },
        { name: 'body', content: 'Main body content' },
        { name: 'footer', content: 'Footer contet' }
      ]
    }
  }
}
</script>

<template>
  <BlogLayout>
    <template 
      v-for="section in layout" 
      v-slot:[section.name]
      :key="`blog-section-${section.name}`"
    >
      {{ section.content }}
    </template>
  </BlogLayout>
</template>
```

## Does slots have a shorthand syntax?

Similar to `v-bind` and `v-on`, `v-slot` also has a shorthand syntax as well: `#`. Using the previous example, this is how the code would look instead.

📄**App.vue**

```html
<template>
  <CustomLayout>
    <template #header>
      <p>Header content</p>
    </template>
    <template>
      <p>Main body content</p>
    </template>
    <template #footer>
      <p>Footer content</p>
    </template>
  </CustomLayout>
</template>
```

However, from my personal experience, I would caution against using this shorthand in your codebase as it can cause confusion with users who are not as familiar with slots. It can easily be mistaken for a CSS ID rather than something specific to the slot API in Vue. So use with caution!

## Let’s Revue

And with that, you can now manage more than one slot while also having the ability to programmatically define slots. In our next lesson, we will take a look at a powerful but complex topic: scoped slots!