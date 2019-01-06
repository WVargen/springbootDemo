<template>
  <div>
  <el-container class="home_container">
    <!--导航-->
    <el-header>
      <div class="home_title">网站主页</div>
      <div class="home_userinfoContainer">
        <el-dropdown @command="handleCommand">
        <span class="el-dropdown-link home_userinfo">
          {{currentUserName}}<i class="el-icon-arrow-down el-icon--right home_userinfo"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="sysMsg">系统消息</el-dropdown-item>
            <el-dropdown-item command="login">登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </div>
    </el-header>
    <!--菜单-->
    <el-container>
      <el-menu
      :default-active="activeIndex2"
      class="el-menu"
      mode="horizontal"
      @select="handleSelect">
        <el-menu-item index="1">首页</el-menu-item>
        <el-submenu index="2">
          <template slot="title">产品展示</template>
          <el-menu-item index="2-1">选项1</el-menu-item>
          <el-menu-item index="2-2">选项2</el-menu-item>
          <el-menu-item index="2-3">选项3</el-menu-item>
          <el-submenu index="2-4">
            <template slot="title">选项4</template>
            <el-menu-item index="2-4-1">选项1</el-menu-item>
            <el-menu-item index="2-4-2">选项2</el-menu-item>
            <el-menu-item index="2-4-3">选项3</el-menu-item>
          </el-submenu>
        </el-submenu>
        <el-menu-item index="3" disabled>消息中心</el-menu-item>
        <el-menu-item index="4">管理</el-menu-item>
      </el-menu>
    </el-container>
  </el-container>
  <login-popup class="login_dialog" v-show="LoginVisible" :visible.sync="LoginVisible"></login-popup>
  </div>
</template>
<script>
  import {getRequest} from '../utils/api'
  import Login from '@/components/Login.vue'
  export default{
    components:{
        'login-popup':Login
    },
    methods: {
      handleCommand(command){
        var _this = this;
        if (command == 'login') {
          //console.log("command login");
          _this.LoginVisible = true;
          //console.log(_this.LoginVisible);
        }
      },
      handleSelect(key, keyPath) {
        console.log(key, keyPath);
      }
    },
    mounted: function () {
      var _this = this;
      getRequest("/currentUserName").then(function (msg) {
        if (!msg.data.status == 'error'){
          _this.currentUserName = msg.data;
        }else{
          _this.currentUserName = '游客';
        }
      }, function (msg) {
        _this.currentUserName = '游客';
      });
    },
    data(){
        return {
            LoginVisible: false,
            currentUserName: ''
        }
    }
  }
</script>
<style>
  .home_container {
    height: 100%;
    position: absolute;
    top: 0px;
    left: 0px;
    width: 100%;
  }

  .login_dialog{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0.3);
  }

  .el-header {
    background-color: #20a0ff;
    color: #333;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .el-main {
    background-color: #fff;
    color: #000;
    text-align: center;
  }

  .home_title {
    color: #fff;
    font-size: 22px;
    display: inline;
  }

  .home_userinfo {
    color: #fff;
    cursor: pointer;
  }

  .home_userinfoContainer {
    display: inline;
    margin-right: 20px;
  }
</style>
