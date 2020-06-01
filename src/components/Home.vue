<template>
  <el-container>
    <el-header>
      <div>
        <img src="../assets/images/logo.jpg" alt />
        <span>后台管理系统</span>
      </div>
      <el-button type="danger" round @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主体区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class='toggle-button' @click='toggleCollapse'>|||</div>
        <!-- 侧边栏菜单区域 -->
        <el-menu background-color="#1dd1a1" text-color="#409EFF"
         active-text-color="blue" unique-opened :collapse="isCollapse"
         :collapse-transition='false' router :default-active="activePath">
          <el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">
            <template slot="title">
              <i :class="iconsObj[item.id]"></i>
              <span>{{item.authName}}</span>
            </template>
            <el-menu-item :index="'/' + subItem.path + ''" v-for='subItem in item.children'
             :key="subItem.id" @click='saveNavState("/" + subItem.path)'>
              <i class="el-icon-menu"></i>
              <span>{{subItem.authName}}</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧内容主体 -->
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data () {
    return {
      // 左侧菜单数据
      menulist: [],
      iconsObj: {
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-shangpin',
        102: 'iconfont icon-danju',
        145: 'iconfont icon-baobiao'
      },
      // 是否折叠
      isCollapse: false,
      activePath: ''
    }
  },
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有数据
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
      // console.log(res)
    },
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    // 保存链接的激活状态
    saveNavState (activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang='less' scoped>
.el-container {
  height: 100%;
}
.el-header {
  background-color: #10ac84;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #fff;
  font-size: 20px;

  > div {
    display: flex;
    align-items: center;
  }

  img {
    width: 56px;
    height: 56px;
    border-radius: 50%;
  }
  span {
    margin-left: 15px;
  }
}
.el-aside {
  background-color: #1dd1a1;
  .el-menu {
    border-right: none;
  }
}
.el-main {
  background-color: #ededed;
}
.iconfont {
  margin-right: 10px;
}
.toggle-button {
  color: #fff;
  background-color: #22a6b3;
  text-align: center;
  font-size: 10px;
  line-height: 24px;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
