<template>
  <div class="user-af-wrapper">
    <div class="af-tabs-nav">
      <router-link :to="'/attention/'+$route.params.uid">TA的关注</router-link>
      <router-link :to="'/fans/'+$route.params.uid">TA的粉丝</router-link>
    </div>
    <!--关注列表-->
    <ul v-if="fansList.list"   class="user-af-list">
      <template v-if="fansList.list.length>0">
        <li v-for="(item,$index) in fansList.list"  class="clear af-list-item">
            <div class="af-item-avatar">
              <router-link :to="'/reader/'+item.userId" :target="'_blank'">
                <img :src="item.userHeadPortraitURL" :alt="item.userName">
                <i v-if="item.vipGrade" class="zdy-icon__user_vip"></i>
              </router-link>
            </div>
            <div class="af-item-main">
              <div class="af-handle fr">
                <span v-if="item.userId!=$store.state.userInfo.userId" class="v-middle zdy-attention__wrap" @click="!item.isfollow?$addAttention($index,'fans'):$cancelAttention($index,'fans')">
                  <i :class="{cancel:!item.isfollow}" class="zdy-icon__attention v-item"></i>关注
                </span>
              </div>
              <div class="af-item-content">
                <p class="user-info">
                  <span class="v-item">{{item.followUserName}}</span>
                  <i class="v-item zdy-icon__sex" :class="item.userSex?'girl':null" ></i>
                  <zdy-icon__user_level :grade="item.userGrade"></zdy-icon__user_level>
                </p>
                <div class="item-context">
                  {{item.userAutograph?item.userAutograph:'ta很懒，没有签名哦！'}}
                </div>
              </div>
            </div>
        </li>
      </template>
      <zdy-hint v-else type="fans"></zdy-hint>
    </ul>
    <el-pagination
      class="pbg"
      v-if="fansList.list && fansList.total>fansList.pageSize"
      @current-change="handleCurrentChange"
      :current-page.sync="fansList.pageNum"
      :page-size="fansList.pageSize"
      layout="prev, pager, next, jumper"
      :total="fansList.total">
    </el-pagination>
  </div>
</template>

<script type="text/ecmascript-6">
  import * as service from '../../api/service'
  export default{
    data(){
      return {
        fansList:{}
      }
    },
    methods:{
      getFansList(page){
          service.FetchGetUserData(page,'fans',this.$route.params.uid).then(json=>{
            if(json.returnCode===200){
              this.fansList = json.data
            }
          });
      },
      handleCurrentChange(page){
          this.getFansList(page)
      }
    },
    mounted(){
      this.getFansList(1)
    }
  }
</script>
<!--<style lang="stylus" type="text/stylus" rel="stylesheet/stylus">-->

<!--</style>-->
