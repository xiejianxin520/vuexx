<template>
  <el-container class="home-wrapper">
    <!-- 头部 -->
    <el-header>
      <el-row>
        <el-col :span="5" class='logo'>
          <img src="@/assets/hanxin.jpg" alt="">
        </el-col>
        <el-col :span="11">
          <h1 class="title">电商后台管理系统</h1>
        </el-col>
        <el-col :span="8">
          <div class="welcome">
            <span>欢迎瀚信会员</span>
            <a href="javascript:;" @click.prevent="logout">退出 </a>
          </div>
        </el-col>
      </el-row>
    </el-header>
    <el-container>
      <!-- 侧边栏 -->
      <el-aside style="width:150px;height:100%">
        <!-- 
        default-active 当前激活菜单的 index 值 与第二菜单相关
        el-submenu 表示一组菜单 el-menu-item表示子菜单  
        template: 用来包裹一级菜单，内部指定菜单的图标和菜单名
        如果要给菜单添加 小图标，应该使用 template 来包裹整个内容
         unique-opened 是否打开一个子菜单
              -->
        <!-- 开启路由配置 :router='true'-->
        <!-- 第一个菜单，index是标识别，与:default-active相关，不能重复 -->
        <!-- 里面的子菜单index可以绑定路由路径跳转 -->
        <!-- 启用路由模式后，index就相当于 原来 router-link 中的to属性，用来指定导航的路径（哈希值） -->
        <!-- 可以使用 /home/users 或者 home/users -->
        <el-menu :router='true' :default-active="$route.path.slice(1).split('-')[0]" unique-opened class="el-menu-vertical-demo" background-color="#545c64" text-color="#fff" active-text-color="#ffd04b">
          <el-submenu v-for="menu in menuList" :key="menu.id" :index="menu.path">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>{{menu.authName}}</span>
            </template>
            <el-menu-item v-for="item in menu.children" :key="item.id" :index="item.path" class='menuitem'>
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>{{item.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
          <!-- 第二个菜单，index唯一 -->
          <!-- 里面的子index可以绑定路由路径跳转 -->
          <!-- <el-submenu index="2">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>权限管理</span>
            </template>
            <el-menu-item index="/home/roles" class='menuitem'>
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>角色列表</span>
              </template>
            </el-menu-item>
            <el-menu-item index="/home/rights" class='menuitem'>
              <template slot="title">
                <i class="el-icon-menu"></i>
                <span>权限列表</span>
              </template>
            </el-menu-item>
          </el-submenu> -->
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
  name: 'Home',
  data() {
    return {
      menuList: []
    }
  },
  methods: {
    logout() {
      this.$confirm('您是否确认退出?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        .then(() => {
          localStorage.removeItem('token')
          this.$router.push('/login')
        })
        .catch(() => {
          this.$message({
            type: 'info',
            message: '已取消退出',
            duration: 1000 //显示1秒
          })
        })
    },
    //获取菜单列表
    async getmenuList() {
      const res = await this.$http.get('/menus')
      let {
        meta: { status },
        data
      } = res.data
      if (status === 200) {
        // console.log('获取菜单列表', data)
        this.menuList = data
      }
    }
  },
  created() {
    this.getmenuList()
  },
  components: {}
}
</script>

<style scope lang="less">
.home-wrapper {
  height: 100%;
  width: 100%;

  .el-header {
    padding: 0;
    background-color: white;
    text-align: center;
    .logo {
      text-align: left;
    }

    .title {
      margin: 0;
      line-height: 60px;
      color: black;
      font-size: 28px;
      text-align: center;
    }

    img {
      height: 52px;
      width: 100px;
    }
    .welcome {
      line-height: 60px;
      font-weight: bold;
      text-align: right;
      padding-right: 30px;

      a {
        color: #b07a17;
        text-decoration: none;
      }
    }
  }

  .el-container {
    height: 100%;
  }
  .el-aside {
    background-color: #d3dce6;
    color: #333;
    line-height: 200px;

    .el-menu-vertical-demo {
      height: 100%;
    }

    .el-menu-item {
      min-width: 137px;
      padding: 0;
    }
  }

  .el-main {
    background-color: #e9eef3;
    color: #333;
  }

  .el-container:nth-child(5) .el-aside,
  .el-container:nth-child(6) .el-aside {
    line-height: 260px;
  }

  .el-container:nth-child(7) .el-aside {
    line-height: 320px;
  }
}
</style>
