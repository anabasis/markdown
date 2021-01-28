# VUE

```html
<template>
  <div id="app">
    <v-app id="inspire" >
      <v-app id="inspire" >

        <!-- 메뉴 LEFT -->
        <v-navigation-drawer app >
        </v-navigation-drawer>
        <!-- 메뉴 LEFT -->
  
        <!-- 상단 TOP -->
        <v-app-bar >
        </v-app-bar>
        <!-- 상단 TOP -->

        <!-- 메인화면 -->
        <v-main>
          <v-container >
              <router-view />
          </v-container>
        </v-main>
        <!-- 메인화면 -->
        
        <!-- FOOTER -->
        <v-footer app>
        </v-footer>  
        <!-- FOOTER -->
      </v-app>
    </v-app>

  </div>
</template>
```

```js
<script>
export default {
  props: {
    key: value
  },
  data: () => ({
    key: value,
    ...
    keys : [
      { key1: value1, key2: value2},
      ...
    ],
  }),
  computed: {
    method_name () {
      return result
    },
    ...
  },
  methods: {
    method_name () {
      ...
    }
  }
}
</script>
```

```css
<script scoped>
.class명 {
    ...
}
</style>
```
