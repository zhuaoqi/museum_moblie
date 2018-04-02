<template>
  <div class="on-data">
    <img :src="icon"/>
    <p class="txt">{{title}}</p>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    props: {
      title: {
        type: String,
        default: '暂无数据'
      }
    },
    data() {
      return {
        icon: '../static/images/loading.png'
      }
    }
  }
</script>
<style lang="less">
  .on-data {
    width: 4rem;
    text-align: center;
    margin: 0 auto;
    margin-top: 1.1rem;
    margin-bottom: 1.1rem;
    img {
      width: 100%;
    }
    .txt {
      font-size: .45rem;
      color: #d2d2d2;
      padding-top: .3rem;
      padding-bottom: .3rem;
    }
  }
</style>
