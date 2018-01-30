<template>
  <div id="app">
    <!-- Popup -->
    <app-popup v-if="window_width < 100"></app-popup>

    <!-- Index -->
    <app-index v-else></app-index>
  </div>
</template>

<script>
  import AppPopup from './Popup.vue'
  import AppIndex from './Index.vue'

  export default {
    name: 'app',
    components: {
      AppPopup,
      AppIndex,
    },
    data() {
      return {
        window_width: window.innerWidth,
      }
    }
  }
</script>

<style lang="scss">

</style>
