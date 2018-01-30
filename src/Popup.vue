<template>
  <div id="popup">

    <template v-if="!group_created">
      <div class="form-group">
        <label for="workspaceName"></label>
        <input v-model="group_name" @keyup.enter="create_group"
               id="workspaceName" class="form-control" type="text""
               autofocus placeholder="save tabgroup as:">
        <br>
        <button @click="create_group" class="btn btn-success">Create</button>
      </div>
    </template>
    <template v-else>
      <p id="success_message">{{this.group_name}} has been created!</p>
    </template>
  </div>
</template>

<script>
  export default {
    name: "popup",
    data() {
      return {
        group_name: '',
        group_created: false,
      }
    },
    methods: {
      create_group(){
        this.group_name = this.group_name.trim()
        if (this.group_name){
          this.group_created = true
          this.add_group()
          // setTimeout(() => window.close(), 1000)
        } else {
          document.getElementById('workspaceName').placeholder = "name can't be empty!"
        }
      },
      add_group(){
        chrome.storage.sync.get({
          groups: {},
          order: [],
        }, data => {
          const groups = data.groups
          const order = data.order

          const curTabs = []

          chrome.tabs.getAllInWindow(tabs => {
            alert(JSON.stringify(tabs))
            curTabs.push(...tabs.map(t => {
              return {url: t.url, favIconUrl: t.favIconUrl}
            }))
            order.push(this.group_name)
            const group = {name: this.group_name, tabs: curTabs}
            groups[this.group_name] = group
            chrome.storage.sync.set({
              groups: groups, order: order,
            });
          })

        })
      },
    },
  }
</script>

<style scoped lang="scss">
  #popup {
    width: 200px;
    height: 200px;
    margin: auto;
    text-align: center;
    font-weight: bolder;

    input {
      text-align: center;
    }

    button {
      width: 100%;
    }

    #success_message {
      line-height: 100px;
    }
  }
</style>
