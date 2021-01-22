# main.js

```js
import Vue from 'vue';
import App from './App.vue';
import { router } from './router';
import store from './store';
import BootstrapVue from 'bootstrap-vue';
import Fabric from 'fabric';
import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap-vue/dist/bootstrap-vue.min.css';
import VeeValidate from 'vee-validate';
import { library } from '@fortawesome/fontawesome-svg-core';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import {
  faHome,
  faUser,
  faUserPlus,
  faSignInAlt,
  faSignOutAlt
} from '@fortawesome/free-solid-svg-icons';
import vuetify from './plugins/vuetify';

library.add(faHome, faUser, faUserPlus, faSignInAlt, faSignOutAlt);
```