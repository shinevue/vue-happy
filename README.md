# illustrate

> A very simple vue2 + vuex project, the whole process is clear at a glance, although the sparrow is small, it has all the internal organs, suitable as an introductory exercise.

> If it is helpful to you, you can click "Star" in the upper right corner to support it. Thank you! ^\_^

> Or you can "follow", I will continue to open source more interesting projects

> If you have any questions, please directly mention them in Issues, or if you find a problem and have a very good solution, welcome to PR 👍

> Development environment macOS 10.12.3 Chrome 56 nodejs 6.10.0

> This project is mainly used for the introductory practice of vue2 + vuex. In addition, a large-scale project with relatively complex vue2 is recommended, covering most of the knowledge points of vuejs. The current project has been completed. [Address here](https://github.com/bailicangdu/vue2-elm)

## Project running (nodejs 6.0+)

```bash
# clone to local
git clone https://github.com/bailicangdu/vue2-happyfri.git

# enter the folder
cd vue2-happyfri

# install dependencies
npm install or yarn (recommended)

# Open the local server localhost:8088
npm run dev

# Release environment
npm run build
```

# Effect demonstration

[demo address](https://cangdu.org/happyfri/) (please use chrome mobile phone mode to preview)

## Routing configuration

```js
import App from "../App";

export default [
  {
    path: "/",
    component: App,
    children: [
      {
        path: "",
        component: (r) =>
          require.ensure([], () => r(require("../page/home")), "home"),
      },
      {
        path: "/item",
        component: (r) =>
          require.ensure([], () => r(require("../page/item")), "item"),
      },
      {
        path: "/score",
        component: (r) =>
          require.ensure([], () => r(require("../page/score")), "score"),
      },
    ],
  },
];
```

## Configure actions

```js
import ajax from "../config/ajax";

export default {
  addNum({ commit, state }, id) {
    //Click on the next question, record the answer id, judge whether it is the last question, if not, skip to the next question
    commit("REMBER_ANSWER", id);
    if (state.itemNum < state.itemDetail.length) {
      commit("ADD_ITEMNUM", 1);
    }
  },
  //Initialization information
  initializeData({ commit }) {
    commit("INITIALIZE_DATA");
  },
};
```

## mutations

```js
const ADD_ITEMNUM = "ADD_ITEMNUM";
const REMBER_ANSWER = "REMBER_ANSWER";
const REMBER_TIME = "REMBER_TIME";
const INITIALIZE_DATA = "INITIALIZE_DATA";
export default {
  //Click to enter the next question
  [ADD_ITEMNUM](state, payload) {
    state.itemNum += payload.num;
  },
  //Record the answer
  [REMBER_ANSWER](state, payload) {
    state.answerid[state.itemNum] = payload.id;
  },
  /*
record time
*/
  [REMBER_TIME](state) {
    state.timer = setInterval(() => {
      state.allTime++;
    }, 1000);
  },
  /*
initialization information,
*/
  [INITIALIZE_DATA](state) {
    state.itemNum = 1;
    state.allTime = 0;
  },
};
```

## create store

```js
import Vue from "vue";
import Vuex from "vuex";
import mutations from "./mutations";
import actions from "./action";

Vue.use(Vuex);

const state = {
  level: "First week",
  itemNum: 1,
  allTime: 0,
  timer: "",
  itemDetail: [],
  answerid: {},
};

export default new Vuex.Store({
  state,
  actions,
  mutations,
});
```

## Create vue instance

```js
import Vue from "vue";
import VueRouter from "vue-router";
import routes from "./router/router";
import store from "./store/";

Vue.use(VueRouter);
const router = new VueRouter({
  routes,
});

new Vue({
  router,
  store,
}).$mount("#app");
```
