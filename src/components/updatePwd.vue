<template>
  <div id="updatePwd">
    <el-form :model="searchFrom" :rules="rules" label-width="80px">
      <el-form-item label="原密码">
        <el-input
          v-model="searchForm.oldPassword"
          placeholder="请输入6位原密码">
        </el-input>
      </el-form-item>
      <el-form-item label="新密码">
        <el-input
          v-model="searchForm.newPassword"
          placeholder="新密码长度必须在6-16之间，且必须包含数字和字母"
          show-password >
        </el-input>
      </el-form-item>
      <el-form-item>
        <el-button
          style="width:100%;padding:11px;"
          type="success"
          @click="updatePwd"
        >修 改 密 码</el-button>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" style="width:100%;padding:11px;" @click="goBack">返 回</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    const validatePass = (rule, value, callback) => {
      var reg = /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9a-zA-Z]{6,}$/ // 密码长度必须在6-16之间，且必须包含数字和字母
      if (value === '' || value === undefined) {
        callback(new Error('请输入密码'))
      } else if (value.length < 6 || value.length > 18) {
        callback(new Error('密码长度必须在6-16之间'))
      } else if (/^[a-z]+$/.test(value) || /^[0-9]+$/.test(value)) {
        callback(new Error('密码必须同时包含数字和字母'))
      } else {
        if (reg.test(value)) {
          callback()
        }
      }
    }
    return {
      searchForm: {
        oldPassword: '',
        newPassword: ''
      },
      rules: {
        oldPassword: [{ required: true, validator: validatePass, trigger: 'blur' }],
        newPassword: [{ required: true, validator: validatePass, trigger: 'blur' }]
      }
    }
  },
  methods: {
    updatePwd () { // 修改密码
      axios.put('/json/user/updatePassword?oldPassword=' + this.searchForm.oldPassword + '&newPassword=' + this.searchForm.newPassword).then((res) => {
        if (res.data.code === 0) {
          this.$message({
            type: 'success',
            message: '修改密码成功，请重新登录'
          })
        }
        setTimeout(() => {
          this.$router.push({name: 'login'})
        }, 1000)
      })
    },
    goBack () {
      this.$router.go(-1)
    }
  }
}
</script>

<style lang="scss" scoped>
#updatePwd{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  padding: 50px 0;
  padding-right: 30px;
  width: 500px;
  background-color: rgba(49,143,254,.4);
  border-radius: 10px;
  box-shadow: 0px 0px 5px lavenderblush;
  .el-form
    .el-form-item:nth-child(1),
    .el-form-item:nth-child(2) {
    margin-bottom: 20px;
  }
}
</style>
