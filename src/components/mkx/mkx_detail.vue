<template>
  <sub-page>
    <div class="edu-detail">
      <back-top title="教育课程详情" backColor="#fff"></back-top>
      <div class="edu-con">
        <div class="movie-img">
          <img :src="details.img"/>
        </div>
        <div class="movie-info">
          <p class="title">
            {{details.title}}
          </p>
          <p class="address">
            <Icon type="ios-location-outline" class="location"></Icon>
            {{details.address}}
          </p>
          <p class="m-time">
            <Icon type="ios-timer-outline" class="time"></Icon>
            {{details.activity_time}}
          </p>
          <p class="classes">
            <span v-for="(item,index) in details.courseclass" :key="index">{{item}}</span>
          </p>
          <p class="intr" v-html="details.introduce"></p>
        </div>

        <div class="book-box" v-if="!token">
          <p class="sub-title">
            <span class="splt"></span>
            活动预约
          </p>
          <p class="s-tit">预约活动场次前，请先确保已成功预定科技馆门票</p>
          <no-data title="请先登录"></no-data>
        </div>

        <div class="book-box" v-if="token && details.state===1">
          <p class="sub-title">
            <span class="splt"></span>
            活动预约
          </p>
          <p class="s-tit">预约活动场次前，请先确保已成功预定科技馆门票</p>
          <div class="book">
            <div class="item-group">
              <label class="lab">日期选择：</label>
              <DatePicker type="date"
                          placeholder="请选择查询日期"
                          @on-change="handleDate"
                          style="width: 5rem">
              </DatePicker>
            </div>
            <div class="item-group">
              <label class="lab">选择场次：</label>
              <ul class="numbers">
                <li
                  :class="{active:current===index}"
                  :style="{cursor:item.is_stop==1?'pointer':'not-allowed'}"
                  @click="item.is_stop===1 ? toggle(index,item.id) : ''"
                  v-for="(item,index) in numbers"
                  :key="index">
                  {{item.sess}}（{{item.determine}}/{{item.qualified}}）
                </li>
              </ul>
            </div>
            <div class="item-group">
              <label class="lab">添加人数：</label>
              <ul class="people">
                <li v-for="(item,index) in number" v-if="item.status">
                  <div class="form-box">
                    <p>
                      <label class="s-lab">姓名：</label>
                      <input type="text" required v-model="item.name" class="inp"/>
                    </p>
                    <p>
                      <label class="s-lab">年龄：</label>
                      <input type="text" required v-model="item.age" class="inp"/>
                    </p>
                  </div>
                  <div class="icon-minus" @click="minusNumbers(index)">
                    <Icon v-if="index!==0" type="ios-minus"></Icon>
                  </div>
                </li>
              </ul>
              <p class="add" @click="addNumbers()">
                <Icon type="ios-plus-outline" class="icon-add"></Icon>
                <span>添加</span>
              </p>
            </div>
            <div class="notice">
              <p>
                <span>*</span>
                1.请准确填写预约人年龄信息，并确认与该活动要求年龄
                是否符合
              </p>
              <p>
                <span>*</span>
                2.本活动将于活动开始前半小时停止预约
              </p>
            </div>
            <p class="book-btn" @click="_bookSumbit()">立即预约</p>
          </div>
        </div>
        <div class="edu-txt">
          <p class="sub-title">
            <span class="splt"></span>
            活动内容
          </p>
          <div class="content" v-html="details.content"></div>
        </div>
      </div>
    </div>
  </sub-page>
</template>
<script type="text/ecmascript-6">
  import subPage from '@/base/sub-page'
  import {getFutureDetail, getNumbers, bookSumbit} from '@/public/js/fetch'
  import BackTop from '@/base/back'
  import {Icon, DatePicker} from 'iview'
  import moment from 'moment'
  import NoData from '@/base/no-data'

  export default {
    components: {
      subPage,
      BackTop,
      Icon,
      DatePicker,
      NoData
    },
    data() {
      return {
        details: '',
        date: '',
        numbers: '',   //场次
        current: -1,
        reser_id: '',  //场次id
        number: [
          {
            name: '',
            age: '',
            status: 1,
            index: 1
          }
        ],
        listNumber: 1,
        token: ''
      }
    },
    created() {
      this._getFutureDetail()
      this._getNumbers()
      this._token()
    },
    computed: {},
    methods: {
      _getFutureDetail() {
        const result = getFutureDetail(this.$route.params.id)
        result.then(res => {
          this.details = res.data
        }).catch(err => {
          console.log(err.response)
        })
      },
      toggle(index, id) {
        this.current = index
        this.reser_id = id
      },
      /**
       * 日期
       * @param date
       */
      handleDate(date) {
        this.date = parseInt(moment(date).format('X')) + 12 * 60 * 60
        this.current = -1
        this.reser_id = ''
        this._getNumbers()
      },
      /**
       * 获取场次
       */
      _getNumbers() {
        const result = getNumbers(
          this.date,
          this.$route.params.id
        )
        result.then(res => {
          this.numbers = res.data
        }).catch(err => {
          if (err.response.status === 401) {
            this.token = ''
          }
        })
      },
      _token() {
        this.token = this.$cookie.get('token')
      },
      /**
       * 删除
       */
      minusNumbers(index) {
        this.number[index].status = 0
      },
      /**
       * 添加人数
       */
      addNumbers() {
        this.listNumber++
        this.number.push(
          {
            name: '',
            age: '',
            status: 1,
            index: 1
          }
        )
      },
      _bookSumbit() {
        const details = []
        if (!this.number[0].age && !this.number[0].name) {
          this.$Message.error({
            duration: 4,
            content: '至少添加一人'
          });
          return
        }
        for (let k in this.number) {
          if (this.number[k].status) {
            details.push(
              {
                age: this.number[k].age,
                name: this.number[k].name
              }
            )
          }
        }

        const result = bookSumbit(
          this.reser_id,
          details
        )
        result.then(res => {
          if (res.status === 0) {
            this._getNumbers()
            this.number = [
              {
                name: '',
                age: '',
                status: 1,
                index: 1
              }
            ]
            this.reser_id = ''
            this.current = -1
          } else {
            this.$Message.error({
              duration: 4,
              content: res.interpret
            });
          }
        }).catch(err => {
          console.log(err.response)
        })
      }
    }
  }
</script>
<style lang="less">
  .edu-detail {
    .edu-con {
      padding: .3rem .3rem .5rem .3rem;
      display: flex;
      display: -webkit-flex;
      flex-direction: column;
      .movie-img {
        width: 100%;
        img {
          width: 100%;
        }
      }
      .movie-info {
        margin-top: .3rem;
        .title {
          font-size: .45rem;
          color: #333;
          margin-bottom: .25rem;
          a {
            color: #333;
          }
          .mold {
            font-size: .38rem;
            background-image: -webkit-linear-gradient(-131deg, #0097E3 -16%, #3BD9D0 100%);
            background-image: -moz-linear-gradient(-131deg, #0097E3 -16%, #3BD9D0 100%);
            background-image: -o-linear-gradient(-131deg, #0097E3 -16%, #3BD9D0 100%);
            background-image: -ms-linear-gradient(-131deg, #0097E3 -16%, #3BD9D0 100%);
            background-image: linear-gradient(-131deg, #0097E3 -16%, #3BD9D0 100%);
            padding: 0 .2rem;
            display: inline-block;
            -webkit-border-radius: 2px;
            -moz-border-radius: 2px;
            border-radius: 2px;
            vertical-align: middle;
            text-align: center;
            color: #fff;
            margin-left: .15rem;
          }
        }
        .address {
          color: #28bbff;
          font-size: .4rem;
          margin-bottom: .25rem;
          .location {
            width: .45rem;
            font-size: .5rem;
            font-weight: bold;
            margin-right: .1rem;
            text-align: center;
          }
        }
        .m-time {
          color: #5e6d84;
          font-size: .4rem;
          margin-bottom: .25rem;
          .time {
            width: .45rem;
            font-size: .5rem;
            font-weight: bold;
            margin-right: .1rem;
            text-align: center;
          }
        }
        .classes {
          display: flex;
          display: -webkit-flex;
          flex-flow: row wrap;
          align-items: center;
          span {
            font-size: .38rem;
            background-image: linear-gradient(-131deg,#0097e3 -16%, #3bd9d0);
            padding: .1rem .2rem;
            margin-right: .3rem;
            -webkit-border-radius: 2px;
            -moz-border-radius: 2px;
            border-radius: 2px;
            text-align: center;
            color: #fff;
            margin-bottom: .3rem;
          }
        }
        .intr {
          font-size: .4rem;
          color: #9a9a9a;
          overflow: hidden;
          -webkit-box-orient: vertical;
          text-align: justify;
          line-height: .6rem;
        }
      }
      .edu-txt {
        margin-top: .5rem;
        .sub-title {
          padding-top: .4rem;
          padding-bottom: .4rem;
          display: flex;
          display: -webkit-flex;
          align-items: center;
          flex-direction: row;
          font-size: .45rem;
          color: #333;
          .splt {
            width: 3px;
            height: .7rem;
            background-image: linear-gradient(90deg,
            #00b3f7 0%,
            #00b2f7 0%,
            #00b1f6 0%,
            #079fe1 100%),
            linear-gradient(
              #228aff,
              #228aff);
            background-blend-mode: normal,
            normal;
            border-radius: 2px;
            margin-right: .2rem;
          }
        }
        .content {

        }
      }
      .book-box {
        margin-top: .5rem;
        .sub-title {
          padding-top: .4rem;
          padding-bottom: .4rem;
          display: flex;
          display: -webkit-flex;
          align-items: center;
          flex-direction: row;
          font-size: .45rem;
          color: #333;
          .splt {
            width: 3px;
            height: .7rem;
            background-image: linear-gradient(90deg,
            #00b3f7 0%,
            #00b2f7 0%,
            #00b1f6 0%,
            #079fe1 100%),
            linear-gradient(
              #228aff,
              #228aff);
            background-blend-mode: normal,
            normal;
            border-radius: 2px;
            margin-right: .2rem;
          }
        }
        .s-tit {
          font-size: .36rem;
          color: #333;
        }
        .book {
          display: flex;
          display: -webkit-flex;
          flex-direction: column;
          .item-group {
            margin-top: .5rem;
            display: flex;
            display: -webkit-flex;
            flex-direction: column;
            .lab {
              font-size: .4rem;
              color: #333;
              margin-bottom: .3rem;
            }
            .numbers {
              display: flex;
              display: -webkit-flex;
              flex-flow: row wrap;
              align-items: center;
              justify-content: space-between;
              li {
                padding: .2rem .15rem;
                background: #ececec;
                color: #999999;
                font-size: .4rem;
                -webkit-border-radius: 2px;
                -moz-border-radius: 2px;
                border-radius: 2px;
                margin-bottom: .15rem;
                letter-spacing: -1px;
                &.active {
                  background-image: linear-gradient(90deg,
                  #00b3f7 0%,
                  #00b2f7 0%,
                  #00b1f6 0%,
                  #21bef8 0%,
                  #41cbfa 0%,
                  #38a6f4 100%),
                  linear-gradient(
                    #f5f5f5,
                    #f5f5f5);
                  color: #fff;
                }
              }
            }
            .people {
              li {
                display: flex;
                display: -webkit-flex;
                flex-flow: row nowrap;
                justify-content: space-between;
                margin-bottom: .3rem;
                .form-box {
                  display: flex;
                  display: -webkit-flex;
                  flex-direction: row;
                  align-items: center;
                  background: #ececec;
                  padding: .2rem .3rem;
                  -webkit-border-radius: 2px;
                  -moz-border-radius: 2px;
                  border-radius: 2px;
                  p {
                    display: flex;
                    display: -webkit-flex;
                    flex-direction: row;
                    align-items: center;
                    margin-right: .2rem;
                    .s-lab {
                      font-size: .4rem;
                      color: #333;
                    }
                    .inp {
                      width: 2.6rem;
                      height: .8rem;
                      line-height: .8rem;
                      text-align: center;
                      background: #fff;
                    }
                    &:last-child {
                      margin-right: 0;
                    }
                  }
                }
                .icon-minus {
                  font-size: .8rem;
                  color: #999;
                }
              }
            }
            .add {
              background: #ececec;
              padding: .2rem .3rem;
              font-size: .4rem;
              display: table;
              margin-top: .3rem;
              -webkit-border-radius: 2px;
              -moz-border-radius: 2px;
              border-radius: 2px;
              .icon-add {
                font-size: .5rem;
                display: inline-block;
                vertical-align: middle;
              }
              span {
                display: inline-block;
                vertical-align: middle;
                margin-left: .1rem;
              }
            }
          }
          .notice {
            margin-top: .4rem;
            font-size: .36rem;
            color: #9b9b9b;
            p {
              margin-bottom: .2rem;
            }
            span {
              color: #ff1010;
            }
          }
          .book-btn {
            padding: .2rem .4rem;
            font-size: .4rem;
            color: #fff;
            background-image: linear-gradient(90deg,
            #00b3f7 0%,
            #00b2f7 0%,
            #00b1f6 0%,
            #21bef8 0%,
            #41cbfa 0%,
            #38a6f4 100%),
            linear-gradient(
              #f5f5f5,
              #f5f5f5);
            -webkit-border-radius: 2px;
            -moz-border-radius: 2px;
            border-radius: 2px;
            margin-top: .4rem;
            display: table;
          }
        }
      }
    }
  }
</style>
