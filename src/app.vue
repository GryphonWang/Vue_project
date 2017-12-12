<template>
  <!-- 此处有且只能有一个根节点 -->
  <div class="container">
    <header class="mui-bar mui-bar-nav">
			<a v-if='isShow' @click='goBack' class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
			<h1 class="mui-title">VueCMS</h1>
		</header>
		<nav class="mui-bar mui-bar-tab">
      <router-link class="mui-tab-item" to="/home">
				<span class="mui-icon mui-icon-home"></span>
				<span class="mui-tab-label">首页</span>
			</router-link>
      <router-link class="mui-tab-item" to="/member">
				<span class="mui-icon mui-icon-email"></span>
				<span class="mui-tab-label">会员</span>
			</router-link>
			<router-link class="mui-tab-item" to="/shopcar">
				<span class="mui-icon mui-icon-contact"><span class="mui-badge">{{count}}</span></span>
				<span class="mui-tab-label">购物车</span>
			</router-link>
			<router-link class="mui-tab-item" to="/search">
				<span class="mui-icon mui-icon-gear"></span>
				<span class="mui-tab-label">搜索</span>
			</router-link>
		</nav>
    
    <router-view></router-view>
  </div>
    
</template>

<script>
import vueObj from './config/communication'
  export default {
    data: function () {
      return {
        isShow: false,
				count:0
      }
    },
		methods:{
			goBack(){
				this.$router.back();
			},
			judgeBack(path){
				let arr=['/home','/member','/shopcar','/search'];
				if(arr.indexOf(path)==-1){
					this.isShow=true;
				}else{
					this.isShow=false;
				}
			}
		},
		created(){
			this.judgeBack(this.$route.path);
			
			// 当组件创建完毕，监听vueObj的 updateBadge事件
			vueObj.$on('updateBadge',function (count) {
				console.log(count);
				this.count=count;
			  })
		},
		// 当路由地址变化的时候。决定后退按钮显示或者隐藏
		// $router  路由对象  
		// $route   路由规则  当前的路由规则 path params
		watch:{
			'$route':function(newValue){
				// 判断当前的路由地址 是否是  /home  /member /shopcar /search
				this.judgeBack(newValue.path);
			}
		}
    
  }
</script>

<style scoped>

</style>


