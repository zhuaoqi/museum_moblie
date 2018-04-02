<template>
  <div class="container">
    <slot></slot>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    data() {
      return {}
    }
  }
</script>
<style lang="less">
  .container {
    height: calc(~"100vh - 2.2rem");
    background: #f8f8f8;
    overflow-y:auto;
    -webkit-overflow-scrolling: touch;
    position: relative;
  }
</style>
