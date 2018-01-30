<template>
  <div>
    <h1>{{ title }}</h1>
    <div class="wrapper">
      <div v-for="group in groups" class="card">
        <div class="card-block">
          <p class="card-title">{{group.name}}</p>
          <div class="icon_row">
            <template v-for="tab in group.tabs">
              <img :src="tab.favIconUrl" alt="">
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "index",
    data() {
      return {
        title: 'My Playspaces',
        groups: {},
        order: [],
      }
    },
    created(){
      chrome.storage.sync.get({
        groups: {},
        order: [],
      }, data => {
        this.groups = data.groups
        this.order = data.order
      })
    },
  }
</script>

<style scoped lang="scss">
  .wrapper {
    display: grid;
    grid-template-columns: repeat(4, 25%);
    text-align: center;
  }
</style>
