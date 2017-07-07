# vue-ya-stash
Yet Another simple stash storage for Vue

## Design Goals

1. Not too complicated
2. Not too simple
3. Try to keep the standard pattern (`props`-`emit`)

#3 is being specially concerned.
As you see from example, one can effortlessly switch between `props-emit` and `stash` model.
Furthermore ways of universal components for two models will be supported. I wish :)   

## Usage

### Setup
```js
import Vue from 'vue'
import YaStash from './vue-ya-stash.js' // Not yet with npm
import stashStore from './stash'

Vue.use(YaStash)

new Vue({
  el: '#app',
  router,
  stashStore,
  template: '<App/>',
  components: { App }
})
```

`./stash/index.js`
```js
import Vue from 'vue'
var stash = {
  user: {
    name: 'Ted',
    email: 'ted@example.com'
  },
  ui: {
    sidebar: {
      visible: true
    }
  }
}

var stashContainer = new Vue({
  data: {
    stash: stash
  }
})

export default stashContainer.stash
```

### Component
```js
Vue.component('user-card', {
    stash: ['user', 'ui'],
    created () {
      // Access
      console.log(this.user.name)
      // Update
      this.$emit('update:user', {...this.user, name: 'Bob'})
      // Patch
      this.$emit('patch:user', this.user, 'name', 'Bob')
    }
});
```

```js
Vue.component('user-card', {
    stash: {
      name: 'user.name',
      sidebar: 'ui.sidebar'
    }
    created () {
      // Access
      console.log(this.name)
      console.log(this.sidebar.visible)
      // Update
      this.$emit('update:name', 'Bob')
      this.$emit('update:sidebar', {...this.sidebar, visible: true})
      // Patch
      this.$emit('patch:sidebar', this.sidebar, 'visible', true)
    }
});
