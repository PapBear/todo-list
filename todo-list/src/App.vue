<template>
  <div id="app">
    <HeaderView></HeaderView>
    <div class="app__container">
      <router-view/>
    </div>
  </div>
</template>

<script>
import HeaderView from "./components/HeaderView.vue";

export default {
  name: 'app',
  components: {
    HeaderView
  },
  data() {
    return {
    }
  }
}

</script>

<style lang="scss">
  @import './assets/scss/style';
</style>
