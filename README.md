# How to import Bootstrap Vue in Nuxt

It's very important to get this done right, otherwise you end up importing the `Bootstrap` core into every component. This would result in the browser having to download and parse the whole `Bootstrap` core again and again, which we want to avoid at every cost.

So, here's how we do it:

## 1. Install Bootstrap Vue

```bash
# With npm
npm install bootstrap-vue --save
```

## 2. Add the bootstrap vue nuxt mdoule.

In `nuxt.config.js`:

```javascript
export default {
  modules: [
    'bootstrap-vue/nuxt',
  ]
}
```

## 3. Exclude compiled `Bootstrap` and `Bootstrap-Vue` Stylesheets.

We want to import the `Bootstrap` and `Bootstrap-Vue` scss files into our project manually, so we can use and overwrite their scss variables and use their mixins.
Since we are importing the core files manually we **MUST** exclude the compiled source files of `Bootstrap` and `Bootstrap-Vue`.


In `nuxt.config.js`:

```javascript
export default {
  bootstrapVue: {
    bootstrapCSS: false,
    bootstrapVueCSS: false
  },
}
```

## 4. Import the `Bootstrap` and `Bootstrap-Vue` code into a global stylesheet. 

Create a file called `global.scss` in your `assets/styles` folder.

In `assets/styles/global.scss`:

```scss
@import '~bootstrap/scss/bootstrap.scss';
@import '~bootstrap-vue/src/index.scss';
```

Add the global stylesheet into `nuxt.config.js`
In `nuxt.config.js`:

```javascript
export default {
  css: [
    { src: 'assets/styles/global.scss', lang: 'scss'},
  ],
}
```

Now we have manually imported `Bootstrap` and `Bootstrap-Vue` into our project and made the styles available globally. If we would import that code in every component, we would also compile the whole `Bootstrap` code again and again, which would result in a heavy assets download for the browser and a bad user experience.

With this configuration though, we're not able to use `Bootstrap` variables and mixins in our components, so let's see how to fix that:

## 5. Import, use and overwrite `Bootstrap` variables and register your own scss ressources for usage in your components:

We need to make `Bootstrap` variables and your own scss ressources available globally in the whole project. We don't want to import them again and again in every component, so we'll use a very handy nuxt module for that task:

```bash
npm i @nuxtjs/style-resources --save
```

In `nuxt.config.js`:
```javascript
export default {
  modules: [
    '@nuxtjs/style-resources',
  ],
}
```

In `assets/styles` create a file called `_variables.scss`.

In `assets/styles/_variables.scss`:

```scss
$primary: #182540;
```

Now you can import your own scss variables AND `Bootstrap` variables, mixins and functions globally into your project like so:

In `nuxt.config.js`:
```javascript
export default {
  styleResources: {
      scss: [
          'assets/styles/_variables.scss',         
          'bootstrap/scss/_functions.scss',
          'bootstrap/scss/_variables.scss',
          'bootstrap/scss/_mixins.scss',
      ]
  },
}
```

You can also add `mixins` or `fonts` if you have any:

In `nuxt.config.js`:
```javascript
export default {
  styleResources: {
      scss: [
          'assets/styles/_variables.scss',   
          'assets/styles/_mixins.scss',             
          'assets/styles/_fonts.scss',                       
          'bootstrap/scss/_functions.scss',
          'bootstrap/scss/_variables.scss',
          'bootstrap/scss/_mixins.scss',
      ]
  },
}
```

Make sure to declare `assets/styles/_variables.scss` **before** you declare `bootstrap/scss/_variables.scss`. This will enable you to overwrite `Bootstrap` scss variables.

Now that you've included your own scss variables and `Bootstrap` scss ressources in the `styleRessources` module, you made them available globally, which means that they're automatically also included in `assets/styles/global.scss`.

## 6. Congrats, you're done!! :D

Now you can simply use scss variables and mixins in your vue components, without extra import statements and most importantly: You are preventing your app from importing the `Bootstrap` core several times!

In your `component.vue`

```vue
<template>
  <div>
    <h1 class="title"></h1>
  </div>
</template>

<script lang="js">
  export default{}
</script>

<style lang="scss" scoped>
  //no need to import global style ressources
  .title{
    color:$primary;
  }
</style>

```


