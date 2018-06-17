<template>
  <div style="text-align: center">
    <text>{{motto}}</text>
    <open-data class="userAvatarUrl" type="userAvatarUrl"></open-data>
    <open-data type="userNickName"></open-data>
  </div>

</template>

<script>
    export default {
      data () {
        return {
          motto: '农场中心'
        }
      }
    }
</script>

<style scoped>
</style>
