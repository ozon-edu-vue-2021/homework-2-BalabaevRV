<template>
  <p tabindex="-1" @click="isSelected = !isSelected" class="listItem" :class="{selected: isSelected}" data-vue-tab><span>{{title}}</span> <span class="targetLink"> ({{targetLink}})</span> <span class="path">{{currentPath}}</span></p>
</template>

<script>

export default {
  name: "linkVue",
  props: {
    title: String,
    currentPath: String,
    targetLink: String
  },
  data () {
    return {
      isSelected: false
    }
  }
}
</script>

<style scoped>
  .listItem {
    font-size: 18px;
    box-sizing: border-box;
    display: flex;
    align-items: center;
    border: 0.5px solid transparent;
    cursor: pointer;
    margin-top: 5px;
    margin-bottom: 5px;
    border:
  }

  .listItem:hover {
    border: 0.5px solid lightblue;
  }

  .listItem::before {
    content: "";
    background-image: url(../assets/link.svg);
    background-repeat: no-repeat;
    background-size: cover;
    width: 48px;
    height: 48px;
    display: block;
  }
  .selected {
    background-color: #e8f7f5;
  }

  .path {
    margin-left: auto;
    display: none;
  }

  .listItem:hover .path {
    display: block;
  }

  .listItem:focus .path {
    display: block;
  }

  .targetLink {
    font-size: 16px;
    white-space: break-spaces;
  }

  .listItem:focus {
    outline-color: lightblue;
  }
</style>