<template>
  <slot-con>
    <back title="个人设置"></back>
    <div class="setting">
      <div class="form-box">
        <div class="form-group">
          <input type="text"
                 name="phone"
                 v-model="nickname"
                 placeholder="请输入用户昵称"/>
          <span class="txt">用户昵称：</span>
        </div>
        <div class="form-group">
          <input type="text"
                 name="phone"
                 v-model="name"
                 placeholder="请输入真实姓名"/>
          <span class="txt">真实姓名：</span>
        </div>
        <div class="form-group">
          <input type="text"
                 name="phone"
                 v-model="email"
                 placeholder="如：xxxx@qq.com"/>
          <span class="txt">联系邮箱：</span>
        </div>
        <div class="form-group">
          <input type="text"
                 name="phone"
                 v-model="school"
                 placeholder="请输入学校"/>
          <span class="txt">毕业学校：</span>
        </div>
        <div class="form-group">
          <input type="text"
                 name="phone"
                 v-model="born"
                 placeholder="如：2018/02/09"/>
          <span class="txt">出生日期：</span>
        </div>
        <div class="form-group">
          <input type="text"
                 name="phone"
                 v-model="card"
                 placeholder="请输入身份证号"/>
          <span class="txt">身份证号：</span>
        </div>
      </div>
      <p class="notice">
        重要提示：请认真正确填写您的身份证号码，这将影响您的取票和预约
      </p>
      <p class="submit" @click="_editUserInfo()">{{load ? '加载中...' : '确认'}}</p>
    </div>
  </slot-con>
</template>
<script type="text/ecmascript-6">
  import slotCon from '@/base/slot-con'
  import Back from '@/base/back'
  import {Icon} from 'iview'
  import {getUserInfo, editUserInfo} from '@/public/js/fetch'

  export default {
    components: {
      slotCon,
      Icon,
      Back
    },
    data() {
      return {
        nickname: '',
        name: '',
        email: '',
        school: '',
        born: '',
        card: '',
        load: false
      }
    },
    created() {
      this._getUserInfo()
    },
    methods: {
      back(){
        this.$router.back()
      },
      _getUserInfo() {
        const result = getUserInfo()
        result.then(res => {
          this.born = res.data.born
          this.card = res.data.card
          this.email = res.data.email
          this.img = res.data.img
          this.name = res.data.name
          this.nickname = res.data.nickname
          this.school = res.data.school
        }).catch(err => {
          if (err.response.status === 401) {
            this.$router.push('/signIn')
          }
        })
      },
      _editUserInfo() {
        this.load = true
        const result = editUserInfo(
          this.name,
          this.nickname,
          this.email,
          this.school,
          this.born,
          this.card
        )
        result.then(res => {
          this.load = false
        }).catch(err => {
          console.log(err.response)
        })
      }
    }
  }
</script>
<style lang="less">
  .setting {
    padding: .3rem .3rem .5rem .3rem;
    .title {
      padding-top: .4rem;
      padding-bottom: .4rem;
      display: flex;
      display: -webkit-flex;
      align-items: center;
      flex-direction: row;
      font-size: .42rem;
      color: #666;
      .icon{
        font-size: .7rem;
        margin-right: .3rem;
      }
    }
    .form-box {
      background: #fff;
      padding: .3rem;
      -webkit-border-radius: 2px;
      -moz-border-radius: 2px;
      border-radius: 2px;
      overflow: hidden;
      margin-top: .3rem;
      .form-group {
        position: relative;
        margin-bottom: .5rem;
        width: 100%;
        input {
          border-bottom: 1px solid #e5e5e5;
          -webkit-transition: all .3s ease-in-out;
          transition: all .3s ease-in-out;
          background: #fff;
          box-shadow: none;
          color: #666;
          font-size: .4rem;
          width: 100%;
          padding: .2rem .4rem .2rem 2.2rem;
          text-align: right;
          &:focus {
            border-bottom: 1px solid #22A0DD;
            -webkit-transition: all .3s ease-in-out;
            transition: all .3s ease-in-out;
          }
        }
        & > .txt {
          position: absolute;
          top: .22rem;
          left: 3px;
          width: 2rem;
          font-size: .4rem;
        }
        label.error.is-visible {
          visibility: visible;
          opacity: 1;
          right: 6px;
          -ms-filter: "alpha(Opacity=100)";
          -webkit-transform: translate(0, 0);
          -ms-transform: translate(0, 0);
          transform: translate(0, 0);
          -webkit-transition: .3s ease-out;
          transition: .3s ease-out;
        }
        label.error {
          position: absolute;
          top: 11px;
          right: -60px;
          padding: 0 8px;
          opacity: 0;
          color: #c33;
          cursor: text;
          -webkit-transform: translate(20px, 0);
          -ms-transform: translate(20px, 0);
          transform: translate(20px, 0);
          -webkit-transition: .25s ease-out;
          transition: .25s ease-out;
        }
        .input-btn {
          width: 100%;
          height: 1rem;
          text-align: center;
          line-height: 1rem;
          font-size: .4rem;
          color: #bfbfbf;
          background-color: #e5e5e5;
          border-radius: 2px;
          margin-top: .3rem;
          letter-spacing: 3px;
          &.active {
            background: rgb(52, 152, 233);
            color: #fff;
          }
        }
        &:last-child {
          margin-bottom: .2rem;
        }
      }
    }
    .notice {
      font-size: .35rem;
      color: #666;
      padding: .3rem;
    }
    .submit {
      width: 100%;
      padding: .25rem;
      font-size: .42rem;
      text-align: center;
      margin-top: .6rem;
      background-image: linear-gradient(135deg, #00d0fb, #00acf6);
      color: #fff;
      -webkit-border-radius: 2px;
      -moz-border-radius: 2px;
      border-radius: 2px;
      overflow: hidden;
    }
  }
</style>
