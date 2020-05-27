<template>
  <div class='login_container'>
      <div class='login_box'>
          <!-- 头像区域 -->
          <div class="avatar_box">
              <img src="../assets/images/login.jpg" alt="">
          </div>
          <!-- 登录表单区域 -->
          <el-form ref='loginFormRef' :model='loginForm' :rules='loginFormRules' label-width="0" class='login_form'>
            <!-- 用户名 -->
            <el-form-item prop='username'>
              <el-input v-model='loginForm.username' prefix-icon="iconfont icon-user"></el-input>
            </el-form-item>
            <!-- 密码 -->
            <el-form-item prop='password'>
              <el-input v-model='loginForm.password' prefix-icon="iconfont icon-3702mima" type='password'></el-input>
            </el-form-item>
            <div class="btns">
                <el-button type="primary" @click='login'>登录</el-button>
                <el-button @click='resetLoginForm'>重置</el-button>
            </div>
          </el-form>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm () {
      // console.log(this)
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        // console.log(valid)
        // eslint-disable-next-line no-useless-return
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        // console.log(res)
        if (res.meta.status !== 200) return this.$message.error('登陆失败')
        this.$message.success('登录成功')
        // console.log(res)
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang='less' scoped>
.login_container {
    background-color: #22a6b3;
    height: 100%;
}
.login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 5px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);

    .avatar_box {
        width: 130px;
        height: 130px;
        border: 1px solid #eee;
        border-radius: 50%;
        padding: 10px;
        box-shadow: 0 0 10px #ddd;
        background-color: #fff;
        position: absolute;
        left: 50%;
        transform: translate(-50%, -50%);

        img {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: #eee;
        }
    }
}
.login_form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px 40px;

    .btns {
        display: flex;
        justify-content: flex-end;
    }
}
</style>
