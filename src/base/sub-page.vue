<template>
  <div class="sub-list">
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
  .sub-list {
    background: #fff;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
    min-height: calc(~"100vh - 2.2rem");
  }
</style>
