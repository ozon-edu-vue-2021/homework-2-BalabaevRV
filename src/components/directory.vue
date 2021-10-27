<template>
  <li>
    <button class="folderButton"  v-if="structure.type === 'directory'" @click="openDirectory = !openDirectory" tabindex="-1" data-vue-tab>
      <span>{{structure.name}}</span>
      <span class="path">{{currentPath}}</span>
    </button>
    <file-vue v-if="structure.type === 'file'" :title="structure.name" :currentPath="currentPath + structure.name"/>
    <link-vue v-if="structure.type === 'link'" :title="structure.name" :currentPath="currentPath + structure.name" :targetLink="structure.target"/>
    <ul v-if="show" :style="{marginLeft: marginLeft + 'px'}">
        <directory-vue
            v-for="(item) in structure.contents"
            :key="item.name"
            :structure="item"
            :marginLeft="marginLeft + 20"
            :currentPath="currentPath + structure.name + '/'"
        />
    </ul>
  </li>
</template>

<script>
  import fileVue from "./file";
  import linkVue from "./link";

  export default {
    name: "directoryVue",
    components: {
      fileVue,
      linkVue
    },
    props: {
      structure: Object,
      type: String,
      title: String,
      Content: Array,
      marginLeft:  {
        type: Number,
        default: 20
      },
      currentPath:  {
        type: String,
        default: "/"
      }
    },
    data: function () {
      return {
        openDirectory: false,
      }
    },
    computed: {
      show () {
        return this.structure.type === "directory" && this.openDirectory;
      }
    }
  }

</script>

<style scoped>

  .folderButton {
    font-weight: 700;
    font-size: 20px;
    background: none;
    margin-top: 10px;
    margin-bottom: 10px;
    display: flex;
    align-items: center;
    cursor: pointer;
    box-sizing: border-box;
    border: 0.5px solid transparent;
    padding: 0;
    width: 100%;
  }

  .folderButton:hover {
    border: 0.5px solid lightblue;
  }

  .folderButton::before {
    content: "";
    background-image: url(../assets/folder.svg);
    background-repeat: no-repeat;
    background-size: cover;
    width: 48px;
    height: 48px;
    display: block;
  }

  .folderButton:focus {
    outline-color: lightblue;
  }

  .path {
    margin-left: auto;
    display: none;
  }

  .folderButton:hover .path{
    display: block;
  }

  .folderButton:focus .path{
    display: block;
  }

</style>