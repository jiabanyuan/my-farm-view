<template>
  <div class="container" >
    <image class="background" mode="widthFix" src="/static/image/bgp.png"/>

    <!--<button class="button" @click="toMyFarm()"></button>-->
    <button class="button" open-type="getUserInfo" @click="toMyFarm()">{{text}}</button>

    <button class="button" open-type="share">{{inviteFriend1}}</button>

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
              console.log(res)
              this.userInfo = res.userInfo
            }
          })
        }
      })
    },
    toMyFarm () {
      let userInfo = '111'
      console.log(userInfo.nickName)
      wx.request({
        url: 'http://localhost:5050/home/index',
        data: {
          nickName: userInfo
        },
        header: {
          'content-type': 'application/json' // 默认值
        },
        success: (res) => {
          console.log(res.data)
        }
      })
    }
  },
  onShareAppMessage (result) {
    let title = 'hello farm'
    let path = '/pages/counter/main'
    this.billId = 'billId-' + new Date().getTime()
    return {
      title,
      path,
      success: async (res) => {
        console.log(`分享成功，此次path为：${path}`)
        await this.$store.dispatch('createBill', { ...this.userInfo, ...this.billInfo })
        // 上传图片
        await this.$store.dispatch('uploadImg', {
          billId: this.billId,
          filePath: '/static/image/background.png'
        })
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
