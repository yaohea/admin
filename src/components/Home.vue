<template>
<el-container class="home-container">
  <el-header>
    <div>
      <img src="../assets/heima.png" alt="">
      <span>药膳管理系统</span>
    </div>
    <el-button type="info" @click="logout">退出</el-button></el-header>
  <el-container>
    <el-aside :width="isCollapse ? '64px' : '200px'">
      <div class="toggle-button" @click="toggleCollapse">|||</div>
      <el-menu background-color="#333744" text-color="#fff" active-text-color="#409BFF" unique-opened :collapse="isCollapse" :collapse-transition="false" router :default-active="activePath">
         <!-- 一级菜单 -->
         <el-submenu :index="item.id+''" v-for="item in menulist" :key="item.id">
            <!-- 一级菜单模板 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconsObj[item.id]"></i>
              <!-- 文本 -->
              <span>{{item.authName}}</span>
            </template>

            <!-- 二级菜单模板 -->
            <el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/' + subItem.path)">
              <template slot="title">
                 <!-- 图标 -->
                 <i class="el-icon-menu"></i>
                 <!-- 文本 -->
                 <span>{{subItem.authName}}</span>
              </template>

            </el-menu-item>
          </el-submenu>
      </el-menu>
    </el-aside>
    <el-main>
      <router-view></router-view>
    </el-main>
  </el-container>
</el-container>
</template>

<script>
export default {
  data() {
    return {
      menulist: [],
      iconsObj: {
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-danju',
        102: 'iconfont icon-shangpin',
        145: 'iconfont icon-baobiao'
      },
      isCollapse: false,
      activePath: ''
    }
  },
  created() {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout() {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList() {
      const { data: res } = await this.$http.get('/menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
    },
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    saveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}
</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}
.el-header {
  display: flex;
  padding-left: 0;
  justify-content: space-between;
  align-items: center;
  background: #373d41;
  font-size: 20px;
   > div {
    display: flex;
    align-items: center;
  }
  span {
    margin-left: 15px;
    color: #fff;
  }
}
.el-aside {
  background: #333744;

  .el-menu {
    border: 0;
  }
}
.el-main {
  background: #eaedf1;
}
.iconfont {
  margin-right: 10px;
}
.toggle-button {
  background-color: #1e1e1e;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: .2em;
  cursor: pointer;
}
</style>
