<template>
  <div id="app">
    <h1>Дерево файлов</h1>
    <ul>
      <directory-vue :structure="items"/>
    </ul>
  </div>
</template>

<script>
import structure from "/public/static/node_modules.json";
import directoryVue from "./components/directory";

export default {
  name: 'App',
  components: {
    directoryVue
  },
  data () {
    return {
      items: structure,
      currentTab: 1
    }
  },
  methods: {
    getNextEl (currentTabEl) {
        let nextTabEl =currentTabEl;
        let lastChild = currentTabEl.parentElement.lastElementChild;

        while (nextTabEl === lastChild) {
          nextTabEl = nextTabEl.parentElement;
          lastChild = nextTabEl.parentElement.lastElementChild;
          if (nextTabEl.tagName === "DIV") {
            return currentTabEl;
          }
        }

      nextTabEl = nextTabEl.nextElementSibling;

        if (nextTabEl.tagName === "LI" || nextTabEl.tagName === "UL") {
          while (nextTabEl.tagName === "LI" || nextTabEl.tagName === "UL") {
            nextTabEl = (nextTabEl.firstElementChild) ? nextTabEl.firstElementChild : this.getNextEl(nextTabEl);
          }
          return nextTabEl;
        }
        return nextTabEl;
    },
    getPreviousEl (currentTabEl) {
      let previousTabEl =currentTabEl;
      let firstChild = currentTabEl.parentElement.firstElementChild;

      while (previousTabEl === firstChild) {
        previousTabEl = previousTabEl.parentElement;
        firstChild = previousTabEl.parentElement.firstElementChild;
        if (previousTabEl.tagName === "DIV") {
          return currentTabEl;
        }
      }

      previousTabEl = previousTabEl.previousElementSibling;

      if (previousTabEl.tagName === "LI" || previousTabEl.tagName === "UL") {
        while (previousTabEl.tagName === "LI" || previousTabEl.tagName === "UL") {
          previousTabEl = (previousTabEl.lastElementChild) ? previousTabEl.lastElementChild : this.getPreviousEl(previousTabEl);
        }
        return previousTabEl;
      }
      return previousTabEl;

    },
    changeFocus (e) {
      if (e.key === "ArrowDown" || e.key === "ArrowUp") {
        let nextTabEl = document.activeElement;
        const getEl = (e.key === "ArrowDown") ? this.getNextEl : this.getPreviousEl;
        if (nextTabEl.hasAttribute("data-vue-tab")) {
            nextTabEl = getEl (nextTabEl);
        } else {
          nextTabEl = document.querySelector("[data-vue-tab]");
        }
        if (nextTabEl) {
          nextTabEl.focus();
        }
      }
    }
  },
  created() {
    document.addEventListener("keydown", this.changeFocus);
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

p {
  margin: 0;
}
</style>
