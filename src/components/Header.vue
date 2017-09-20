<template>
  <div class="container">
    <el-menu :router="true" :default-active="active" class="el-menu" mode="horizontal" @select="handleSelect">
      <el-menu-item class="logo" index="/">番薯</el-menu-item>
      <el-menu-item index="/list"><i class="fa fa-flag" aria-hidden="true">探索</i></el-menu-item>
      <template v-if="user">
        <li class="el-menu-item right" @click="heandleExit"><i class="fa fa-sign-out" aria-hidden="true"></i> 注销</li>        
        <el-submenu index="5" class="right">
          <span slot="title"> {{ user.getUsername() }} </span>
          <el-menu-item index="5-1">个人中心</el-menu-item>
          <el-menu-item index="5-2">发布文章</el-menu-item>
          <el-menu-item index="5-3">消息</el-menu-item>
        </el-submenu>
      </template>
      <template v-else>
        <el-menu-item index="/signUp" class="right"><i class="fa fa-user-o" aria-hidden="true">注册</i></el-menu-item>
        <el-menu-item index="3" class="right"><i class="fa fa-key" aria-hidden="true">登录</i></el-menu-item>
      </template>
    </el-menu>
  </div>
</template>
<script>
import { mapState, mapActions } from 'vuex'
export default {
  name: 'Header',
  data() {
    return {
      active: '/'
    };
  },
  computed: mapState(['user']),
  created(){
    this.active = this.$route.path;   //解决刷新高亮
    console.log('this.$route.path: ')
    console.log(this.$route.path)
    this.$router.afterEach((to, from) => {
      this.active = to.path;    // 解决切换路由高亮
      console.log('to.path: ')
      console.log(to.path)
    })
  },
  methods: {
    handleSelect(key, keyPath) {
      console.log(key, keyPath)
    },
    ...mapActions(['exit']),
    heandleExit(){
      this.exit();
      this.$api.SDK.User.logOut()  //SDK的退出
      this.$message.success("成功退出");
    }
  }
}
</script>
<style lang="css" scoped>
.container{
  padding: 0 10%;
  background: #eef1f6;
}
.right{
  float: right;
}

.logo:first-child{
  margin-left: 0;
  font-size: 25px;
  font-weight: 100;
  background: #20a0ff;
  color: #fff;
}

.logo:first-child:hover{
  background: #20a0ff;
}
</style>