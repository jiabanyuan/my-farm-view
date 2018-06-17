<template>
  <div class="container" >
    <image class="background" src="/static/image/first-page.png"/>
    <div class="page-button" >
        <button class="button" @click="toMyFarm()">{{text}}</button>
        <button class="button" open-type="share">{{inviteFriend1}}</button>
    </div>
  </div>
</template>

<script>
import card from '@/components/card'

export default {
  data () {
    return {
      motto: 'Hello Farm',
      userInfo: {},
      text: '进入应用',
      inviteFriend1: '邀请好友',
      billId: ''
    }
  },

  components: {
    card
  },
  computed: {
    billInfo () {
      return {
        time: this.time,
        from: this.from,
        to: this.to,
        billId: this.billId
      }
    }},

  methods: {
    bindViewTap () {
      const url = '../logs/main'
      wx.navigateTo({ url })
    },
    getUserInfo () {
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.rawData
            }
          })
        }
      })
    },
    toMyFarm () {
      var userInfoStr = this.userInfo
      wx.request({
        url: 'http://localhost:5050/home/index',
        method: 'post',
        dataType: 'json',
        data: JSON.parse(userInfoStr),
        header: {
          'content-type': 'application/json' // 默认值
        },
        success: (res) => {
          console.log(res.data)
        }
      })
    },
    bindGetUserInfo (e) {
      console.log(e.detail.userInfo)
    }
  },
  onShareAppMessage (result) {
    let title = 'hello farm'
    let path = '/pages/index/main'
    this.billId = 'billId-' + new Date().getTime()
    return {
      title,
      path,
      success: async (res) => {
        console.log(`分享成功，此次path为：${path}`)
        await this.$store.dispatch('createBill', { ...this.userInfo, ...this.billInfo })
        wx.redirectTo(`../join/main?billId=${this.billId}`)
      }
    }
  },
  created () {
    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
  }
}
</script>

<style scoped>

.button {
  background-color: #4CAF50; /* Green */
  border: none;
  color: white;
  width: 200px;
  height: 40px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin-top: 20px;
  top: 50%;

}

.page-button {
  height: 100vh;
  text-align: center;
  position: relative;
}

.background {
  height: 100vh;
  width: 100%;
  position: absolute;
}

.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
}

.counter {
  display: inline-block;
  margin: 10px auto;
  padding: 5px 10px;
  color: blue;
  border: 1px solid blue;
}
</style>
