<template>
  <li class="news-item">
    <p class="date">
      <span class="day">{{dateStr.slice(-2)}}</span>
      <span class="month">{{dateStr.slice(0, -3)}}</span>
    </p>
    <router-link :to="'/flow_science/'+data.id" class="info">
      <span class="info-title">{{data.title}}</span>
      <span class="intr">{{data.introduce}}</span>
    </router-link>
  </li>
</template>
<script type="text/ecmascript-6">
  import moment from 'moment'

  export default {
    props: {
      data: {
        type: Object,
        default: null
      }
    },
    data() {
      return {}
    },
    computed: {
      dateStr() {
        return moment.unix(this.data.addtime).format('YYYY.MM.DD')
      }
    }
  }
</script>
<style lang="less">
  .news-item {
    display: flex;
    display: -webkit-flex;
    flex-direction: row;
    align-items: center;
    background: #fff;
    -webkit-border-radius: 2px;
    -moz-border-radius: 2px;
    border-radius: 2px;
    box-shadow: 0 3px 36px 0 #ededed;
    margin-top: .4rem;
    padding: .4rem .3rem;
    .date {
      color: #079fe1;
      padding-right: .3rem;
      display: flex;
      flex-direction: column;
      align-items: center;
      .day {
        font-size: .7rem;
      }
      .month {
        font-size: .4rem;
      }
    }
    .info {
      display: flex;
      display: -webkit-flex;
      flex-direction: column;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      text-decoration: none;
      .info-title {
        color: #333;
        font-size: .45rem;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }
      .intr {
        color: #666;
        font-size: .4rem;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        margin-top: .1rem;
      }
    }
  }
</style>
