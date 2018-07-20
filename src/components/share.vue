<template>
	<div id="share" v-show="share_show">
		<div class="mask" v-on:click="change_share()"></div>
		<div class="fenxiangzhiyin">
			<div class="mui-text-right"><i class="icon iconfont icon-fenxiangzhiyin"></i></div>
			<div>分享领红包</div>
		</div>
		<div class="content_1">
			<img src="@/assets/img/874305743360781155.png"/>
			<div class="text_1">
				<!--<button class="share"  @click="pengyou">
					立即分享
				</button>-->
				<!--<span>每天限领一次</span>-->
			</div>
		</div>
	</div>
</template>

<script>
	import $ from "jquery"
	import {ajaxs,http} from "../assets/fc.js";
	export default{
		props: ['zidingyi'],	//接收父级传递的参数
		name:"share",
		components:{
			
		},
		data:function(){
			return{
				model_type:'',
				share_show:false,	//是否显示分享
				userId:localStorage.getItem('ids') && localStorage.getItem('ids')!='undefined' ? localStorage.getItem('ids') : '',
				share_obj:{},		//分享对象
				openid:localStorage.getItem('openid') ? localStorage.getItem('openid') : ''
			}
		},
		methods:{
			change_share(){
				this.share_show=false;
				this.$emit('shoudao',this.share_show);
			},
			shareGetDb_return(x){
				console.log('分享得现金券信息接口',x)
				if(x.code==10000){
					this.share_obj=x.data;
					this.share();
				}else{
//					mui.toast(x.description,{ duration:2000, type:'div' })
				}
			},
			getDB(x){
//				alert(JSON.stringify(x))
//				{'code':666,data:0,'description':'具体内容','status':'success'}
//				mui.toast(x.description,{duration:2000, type:'div' });
				mui.alert(x.description,'提示',function() {
					location.reload();
				},function(){}, 'div');
//				console.log(x);
			},
			share(){
				var this_1=this;
				wx.ready(function(){
//					mui.toast("微信准备就绪!")
					//分享到朋友圈
					wx.onMenuShareTimeline({
			            title: this_1.share_obj.title, 	// 分享标题
			            link: 'http://m.hzjifen.com/shops/dist/index.html#/RedEnvelopes?Inviterid='+this_1.userId, // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
			            imgUrl:this_1.share_obj.imageUrl, 	// 分享图标
			            success: function (res){
							// 用户点击了分享后执行的回调函数
//							alert(this_1.userId)
							http('post','/platform/zlShareGetDb/getDB',{'uId':this_1.userId},this_1.getDB);
			            },
			            cancel: function (res){
			            	mui.toast("分享失败，请重试！")
			        	}
				    });
				    //分享给朋友
				    wx.onMenuShareAppMessage({
						title: this_1.share_obj.title, // 分享标题
						desc: this_1.share_obj.title, // 分享描述
						link: 'http://m.hzjifen.com/shops/dist/index.html#/RedEnvelopes?Inviterid='+this_1.userId, // 分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
						imgUrl: this_1.share_obj.imageUrl, // 分享图标
						type: '', // 分享类型,music、video或link，不填默认为link
						dataUrl: '', // 如果type是music或video，则要提供数据链接，默认为空
						success: function () {
							//用户点击了分享后执行的回调函数
//							alert(this_1.userId)
							http('post','/platform/zlShareGetDb/getDB',{'uId':this_1.userId},this_1.getDB);
						}
					});
				});
			},
			
		},
		mounted:function(){
			
		},
		created:function(){
			var this_1=this;
//			if(this.userId==''){
//				this.$router.push('/login');
//				return;
//			}
//			//分享得现金券信息接口
			console.log(this.userId)
			http('post','/platform/zlShareGetDb/shareGetDb',{'uId':this.userId},this.shareGetDb_return);
			
			var jsApiList=[
					'onMenuShareTimeline',	//分享到朋友圈
					'onMenuShareAppMessage',	//分享给朋友
//					'onMenuShareQQ',		//分享到QQ
//					'onMenuShareWeibo',		//分享到腾讯微博
//					'onMenuShareQZone',	//分享到QQ空间
//					'startRecord',		// 音频接口开始录音接口
//					'stopRecord',		//音频接口停止录音接口
//					'onVoiceRecordEnd',	//监听录音自动停止接口
//					'playVoice',		//播放语音接口		
//					'pauseVoice',		//暂停播放接口
//					'stopVoice',		//停止播放接口
//					'onVoicePlayEnd',	//监听语音播放完毕接口
//					'uploadVoice',		//上传语音接口
//					'downloadVoice',	//下载语音接口
//					'chooseImage',		//没法对本地选择的图片进行预览（chooseImage接口本身就支持预览，不需要额外支持）		
					'previewImage',		//预览图片接口
					'uploadImage',		//上传图片接口
					'downloadImage',	//下载图片接口
//					'translateVoice',	//智能接口	识别音频并返回识别结果接口
//					'getNetworkType',	//设备信息 获取网络状态接口
//					'openLocation',		//地理位置 使用微信内置地图查看位置接口
//					'getLocation',		//获取地理位置接口
//					'hideOptionMenu',	//不知道什么接口
//					'showOptionMenu',	//不知道什么接口
//					'hideMenuItems',	//批量隐藏功能按钮接口
//					'showMenuItems',	//批量显示功能按钮接口
//					'hideAllNonBaseMenuItem',	//隐藏所有非基础按钮接口
//					'showAllNonBaseMenuItem',	//显示所有功能按钮接口
//					'closeWindow',		//关闭当前网页窗口接口
					'scanQRCode',		//调起微信扫一扫接口
					'chooseWXPay',		//发起一个微信支付请求
//					'openProductSpecificView',	//跳转微信商品页接口
//					'addCard',			//批量添加卡券接口
//					'chooseCard',		//拉取适用卡券列表并获取用户选择信息
//					'openCard'			//查看微信卡包中的卡券接口
			]
			
		},
		watch:{
			//这个是监听事件可以不写也能收到数据
			zidingyi:function(x){
				console.log('share.vue收到数据',x);
				this.share_show=x;
			}
		}
	}
</script>

<style>
	#share .fenxiangzhiyin{
		position: absolute;
    	top: 10px;
    	right: 15px;
    	color: #ffffff;
    	animation: fenxiangzhiyin 1s linear infinite ;
	}
	#share .fenxiangzhiyin i{
		font-size: 0.8rem;
	}
	@keyframes fenxiangzhiyin{
		from{opacity: 0;}
		to{opacity: 1;}
	}
	
	
	#share{
		width: 100%;
		height: 100%;
		position: fixed;
		top: 0px;
		left: 0px;
		z-index: 2;
		display: flex;
	}
	#share .mask{
		width: 100%;
		height: 100%;
		background: rgba(0,0,0,0.5);
		position: absolute;
		top: 0px;
		left: 0px;
	}
	#share .content_1{
		/*background: none;*/
		position: relative;
		z-index: 1;
		margin: auto;
		top: -40px;
	}
	#share .content_1>img{
		width: 100%;
		object-fit: cover;
	}
	#share .content_1 .text_1{
		position: absolute;
		/*top: 440px;*/
		left: 0px;
		right: 0px;
		bottom: 0.7rem;
		text-align: center;
		width: 70%;
		display: flex;
		justify-content: center;
		margin: 0px auto ;
		background: #fb4d32;
	}
	#share .content_1 .text_1 .share{
		padding: 0px;
		border: none;
		display: inline-block;
		background: url(../assets/img/246960216428256640.png);
		background-size:cover;
		/*width: 125px;*/
		width: 2.5rem;
		height: 0.8rem;
		line-height: 0.8rem;
		text-align: center;
		color: #FA4534;
	}
	#share .content_1 .text_1 span{
		font-size: 0.26rem;
		padding-top: 0.3rem;
		color: #FFFFFF;
		margin-left: 0.2rem;
	}
	
</style>