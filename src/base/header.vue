<template>
  <div class="header">
    <div class="logo">
      <router-link to="/">
        <img :src="logo"/>
      </router-link>
    </div>
    <div class="icons">
      <router-link to="/user_center" class="user-img">
        <img src="../assets/user-header.png"/>
      </router-link>
      <p class="menu" @click="openSide">
        <img src="../assets/menu.png"/>
      </p>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    props: {
      logo: {
        type: String,
        default: ''
      }
    },
    data() {
      return {}
    },
    methods: {
      openSide() {
        this.$emit('openSide')
      },
      handleUserClick() {
        this.$emit('handleUserClick')
      }
    }
  }
</script>
<style lang="less" scoped>
  .header {
    position: sticky;
    top: 0;
    height: 2.2rem;
    left: 0;
    right: 0;
    background: #fff;
    display: flex;
    display: -webkit-flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 .5rem;
    z-index: 110;
    box-shadow: 0 0 12px #525252;
    .logo {
      width: 4.3rem;
      img {
        width: 100%;
      }
    }
    .icons {
      width: 2rem;
      display: flex;
      display: -webkit-flex;
      justify-content: space-between;
      align-items: center;
      .user-img {
        width: .7rem;
        img {
          width: 100%;
        }
      }
      .menu {
        width: .7rem;
        img {
          width: 100%;
        }
      }
    }
  }
</style>
