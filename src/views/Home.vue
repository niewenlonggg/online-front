<template>
  <q-scroll-area style="position: absolute; height: 100%; width: 100%;">
  <!--首页大致框架-->
  <div class="home">

    <q-action-sheet
            v-model="actionSheet"
            title="Action Sheet"
            @ok="onOk"
            @cancel="onCancel"
            @show="onShow"
            @hide="onHide"
            :actions="[
      {
        label: 'Delete',
        icon: 'delete',
        color: 'red',
        handler: deleteAction
      },
      {
        label: 'Share',
        icon: 'share',
        color: 'primary'
      },

      // 可选的分隔
      {},

      // 继续其他操作
      {
        label: 'Play',
        icon: 'gamepad'
      },
      {
        label: 'Favorite',
        icon: 'favorite'
      }
    ]"
    />
    <q-layout>

      <!--ToolBar-->
      <q-layout-header color="secondary">

        <div class="row justify-center">
          <div class="col-11">
            <q-toolbar color="fontcolor" inverted style="height: 64px;" >

              <i class="fab fa-phoenix-squadron" @click="jumpMain" style="color: #fd7e14; font-size: 35px;cursor: pointer"></i>

              <q-toolbar-title>
                <strong style="color: #5e5e5e; cursor: pointer" @click="jumpMain">酸奶在线网课</strong>
              </q-toolbar-title>


              <q-btn flat round dense icon="create" color="tertiary" @click="jumpEditor" size="18px"/>

              <div style="margin-left: 10px">
                <q-popover style="width: 250px; margin-top: 13px;" anchor="bottom middle" self="top middle" >
                  <!--
                    构成弹出框的DOM元素，
                    在此例中是一个列表：
                  -->
                  <q-card  >
                    <q-card-media>
                      <img  :src="$store.getters.getUserAvatar">
                    </q-card-media>
                    <q-card-title class="text-center ">
                      万千城市
                    </q-card-title>
                    <q-card-separator />
                    <q-card-actions align="center">

                        <q-btn flat color="warning" label="个人中心" @click="openUserLayout" />
                        <q-btn flat color="secondary" @click="logout" label="安全退出" />

                    </q-card-actions>
                  </q-card>
                </q-popover>
                  <img class="home-img" style="width: 40px; height: 40px;" :src="$store.getters.getUserAvatar"/>
              </div>
            </q-toolbar>
          </div>
        </div>

      </q-layout-header>


      <!--main 展示内容(轮播图，最新课程，最热课程)-->
      <q-page-container>

        <router-view/>
      </q-page-container>


      <!--footer-->
      <q-layout-footer class="text-center" style=" background: #303133; box-shadow:none; padding: 20px ; color: #fff;">
        <!--聂文龙 Copyright     &nbsp;&nbsp;©2019-2020-->

        <div class="row justify-center text-center">
          <div class="col-5">
            © 2019 - 2020 &nbsp;&nbsp;&nbsp;niewenlong.cn &nbsp;&nbsp; 晋ICP备 &nbsp;&nbsp; 19003853号-1
          </div>

        </div>

        <div class="row justify-center text-center " style="margin-top: 10px; margin-bottom:10px">
          <div class="col-5">
            <div style="font-size: 35px" class="">
              <q-icon name="fab fa-github" />
            </div>
          </div>
        </div>
      </q-layout-footer>


    </q-layout>
  </div>
  </q-scroll-area>
</template>

<style>

  .home{
    background-image: url('https://class.imooc.com//static/module/index/img/diff-bg.png')
  }
  .home .q-if-standard.q-if-has-label{
    padding-top: 0px;
  }

  .home-img{
    border-radius: 50%;
    cursor: pointer;
    box-shadow: 0px 0px 8px 2px rgba(0,0,0,0.2);
  }
</style>

<script>

import Main from "@/layouts/Main";
import E from 'wangeditor'

export default {

  name: 'PageHome',
  components: {Main},
  data(){
    return{
      searchValue:'',
      actionSheet: false
    }
  },

  methods:{

    jumpMain(){
      this.$router.push("/")
    },
    jumpEditor(){
      this.$router.push('/editor')
    },
    logout(){
      this.$axios({
        method:'get',
        url:'/api/login/logout'
      }).then(response=>{
        if(response.data.success){
          localStorage.removeItem("cookie");
          this.$router.push("/login")
          this.$q.notify({
            type: 'positive',
            timeout: 1000,
            position: 'top',
            message:"退出登录"
          })
        }
      })
    },
    deleteAction () {
      // @ok事件仍将被触发
      this.$q.notify('Deleting...')
    },

    // 用户选择一个操作
    onOk (item) {
      if (item.handler) {
        // 如果我们已经触发了一个处理程序
        return
      }
      this.$q.notify({
        color: 'secondary',
        message: `Clicked on "${item.label}"`
      })
    },

    // 用户取消了操作表
    onCancel () {
      this.$q.notify({
        color: 'tertiary',
        icon: 'done',
        message: 'Action Sheet was dismissed'
      })
    },

    // 当我们展示给用户时
    onShow () { },

    // 当它被隐藏时
    onHide () { },


    //打开个人中心
    openUserLayout(){
      this.$router.push("/userLayout")
    }
  }
}
</script>
