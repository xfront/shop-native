<!--
    
    @author herbluo
    change logs:
    2017/4/12 herbluo created
-->
<template>
  <div>
    <text>Car2</text>
    <tab-bar class="tab-bar" routerName="HomeCar"></tab-bar>
  </div>
</template>

<style scoped>
  .tab-bar {
    position: fixed;

    left: 0;
    bottom: 0;
    height: 108px;
    width: 750px;
  }
</style>

<script>
  import TabBar from '../components/common/TabBar.vue'

  export default {
    components: {TabBar},
    name: 'car'
  }
</script>

<style></style>