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
      let nextTabEl = currentTabEl.nextElementSibling;
      if (!nextTabEl) {
        nextTabEl = currentTabEl.parentElement;
        if (nextTabEl.tagName === "LI") {
          nextTabEl = nextTabEl.nextElementSibling;
        }
        if (!nextTabEl) {
          nextTabEl = currentTabEl.parentElement.parentElement.parentElement.nextElementSibling;
        }
        if (!nextTabEl) {
          return nextTabEl;
        }
      }
      if (nextTabEl.tagName === "UL") {
        if (!(nextTabEl.style.display === "none")) {
          nextTabEl = nextTabEl.firstElementChild;
        } else {
          nextTabEl = nextTabEl.parentElement.nextElementSibling;
        }
      }
      if(!nextTabEl) {
        return nextTabEl;
      }
      if (nextTabEl.tagName === "LI") {
        nextTabEl = nextTabEl.firstElementChild;
      }
      return nextTabEl;
    },
    getPreviousEl (currentTabEl) {
      let previousTabEl = currentTabEl.parentElement.previousElementSibling;
      if (!previousTabEl) {
        previousTabEl = currentTabEl.parentElement.parentElement.previousElementSibling;
      }
      if (previousTabEl.tagName === "H1") {
        return currentTabEl;
      }
      if (previousTabEl.tagName === "LI") {
        previousTabEl = previousTabEl.lastElementChild;
      }
      if (previousTabEl.tagName === "UL") {
        if (!(previousTabEl.style.display === "none")) {
          previousTabEl = previousTabEl.lastElementChild.lastElementChild;
        }
        else {
          previousTabEl = previousTabEl.previousElementSibling;
        }
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
