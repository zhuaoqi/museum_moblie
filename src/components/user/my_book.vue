<template>
  <slot-con>
    <div class="mybook-con">
      <back title="我的预约"></back>
      <div class="mybook">
        <tab-bar :data="navBar" @handleClick="handleType"></tab-bar>
        <ul class="book-list">
          <li v-for="(item,index) in reserlists" :key="index">
            <div class="act-img">
              <img :src="item.img"/>
              <div class="mask" v-if="type===1">
                <div class="mask-con">
                  <p class="mask-txt">
                    <router-link :to="'mkx'+item.future_id" class="look">查看详情</router-link>
                  </p>
                </div>
              </div>
            </div>
            <div class="act-info">
              <div class="time" :style="{color:type===2 ? '#666' : '#05afee'}">
                <p class="date">{{item.sess}}</p>
                <p>{{item.sesstime}}</p>
              </div>
              <div class="act-name">
                <p class="act-title">{{item.title}}</p>
                <p class="number">人数{{item.count}}人</p>
              </div>
            </div>
          </li>
        </ul>
        <no-data title="暂无预约数据" v-if="!reserlists.length"></no-data>
      </div>
    </div>
  </slot-con>
</template>
<script type="text/ecmascript-6">
  import slotCon from '@/base/slot-con'
  import NoData from '@/base/no-data'
  import {Icon} from 'iview'
  import {Reserlists} from '@/public/js/fetch'
  import Back from '@/base/back'
  import TabBar from '@/base/tabBar'

  export default {
    components: {
      slotCon,
      NoData,
      Icon,
      Back,
      TabBar
    },
    data() {
      return {
        reserlists: '',
        type: 1,
        navBar: [
          {title: '即将进行', id: 1},
          {title: '已经结束', id: 2}
        ],
      }
    },
    created() {
      this._Reserlists()
    },
    methods: {
      handleType(id) {
        this.type = id
        this._Reserlists()
      },
      _Reserlists() {
        const result = Reserlists(this.type)
        result.then(res => {
          console.log(res)
          this.reserlists = res.data
        }).catch(err => {
          console.log(err.response)
        })
      }
    }
  }
</script>
<style lang="less">
  .mybook {
    padding: .3rem;
    .book-list {
      width: 100%;
      li {
        display: flex;
        display: -webkit-flex;
        flex-direction: column;
        -webkit-border-radius: 2px;
        overflow: hidden;
        -moz-border-radius: 2px;
        border-radius: 2px;
        margin-bottom: .3rem;
        .act-img {
          position: relative;
          width: 100%;
          height: 5rem;
          overflow: hidden;
          img {
            width: 100%;
          }
          .mask {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, .4);
            .mask-con {
              width: 100%;
              height: 100%;
              display: flex;
              display: -webkit-flex;
              align-items: center;
              justify-content: center;
              .mask-txt {
                padding: .25rem .4rem;
                font-size: .43rem;
                background-image: linear-gradient(135deg, #00d0fb, #00acf6);
                -webkit-border-radius: 2px;
                overflow: hidden;
                -moz-border-radius: 2px;
                border-radius: 2px;
                a {
                  text-decoration: none;
                  color: #fff;
                }
              }
            }
          }
        }
        .act-info {
          display: flex;
          display: -webkit-flex;
          flex-flow: row nowrap;
          padding: .3rem;
          background: #fff;
          .time {
            display: flex;
            display: -webkit-flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: #05afee;
            font-size: .4rem;
            border-right: 1px solid #d4d4d4;
            padding: 0 .4rem;
            .date {
              font-size: .6rem;
              margin-bottom: .1rem;
            }
          }
          .act-name {
            display: flex;
            display: -webkit-flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            color: #333;
            font-size: .4rem;
            padding: 0 .2rem;
            flex: 1;
            .act-title {
              font-size: .8rem;
              margin-bottom: .1rem;
              overflow: hidden;
              text-overflow: ellipsis;
              white-space: nowrap;
            }
          }
        }
      }
    }
  }
</style>
