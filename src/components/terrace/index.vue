<template>
	<div>
		<div id="index">
			<header>
				<span @click="go"><</span>
				平台通知
			</header>
			<nav>
				<ul class="clear">
								
				<router-link to="/terrace/wuliu">
					<li>
						<i :class="{active:nowpath=='wuliu'}" @click="nowpath='wuliu'">快递</i>
					</li>	
				</router-link>
				<router-link to="/terrace/tongzhi">
					<li>
						<i :class="{active:nowpath=='tongzhi'}" @click="nowpath='tongzhi'">通知</i>
					</li>	
				</router-link>
					<li @click="clickFn">
						<i>好友</i>
					</li>
				</ul>
			</nav>
		</div>
		<router-view class="top"></router-view>
	</div>
	
</template>

<script>
	export default{
		components:{},
		data:function(){
			return{nowpath:'wuliu'}
		},
		methods:{
			go:function(){
				this.$router.push({path:"/Personal"})
			},	
			clickFn:function(){
	    		this.$router.push({path:'/xiaoxi'});
			}
		},
		mounted:function(){
					var a=this.$route.path;
					this.nowpath=a.substring(9);
					console.log(this.nowpath)
		},


	}
	
</script>

<style>
	body{background-color:#f9f9f9;}
	#index .active{border-bottom:4px solid #fb5000;color:#fb5000;}
	#index header{width:100%;height:0.87rem;background-color:#00a2ea;text-align: center;line-height:0.87rem;color: white;font-size:0.35rem;position: fixed;top:0;left:0;}
	#index header span{float: left;margin-left:0.3rem;font-size:0.5rem;}
	#index nav{position: fixed;left: 0;top:0.87rem;width:100%;}
	#index nav ul{background-color: #ffffff;width:100%;}
	#index nav ul li{float: left;width:calc(100% / 3);text-align:center;color:#6e6e6e;height:0.9rem;line-height: 0.9rem;padding: 0 0.35rem;}
	#index nav ul li i{display: block;font-size:0.35rem;}
	#index .top{margin-top:calc(0.87rem + 1.2rem );}
	
	
	
</style>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    