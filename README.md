# How to import Bootstrap Vue in Nuxt

It's very important to get this done right, otherwise you end up importing the bootstrap core into every component. This would result in the browser having to download and parse the whole bootstrap core again and again, which we want to avoid at every cost.

So, here's how we do it:

1. Install Bootstrap Vue

```bash
# With npm
npm install bootstrap-vue

# With yarn
yarn add bootstrap-vue
```

2. Add the bootstrap vue nuxt mdoule.

In `nuxt.config.js`
