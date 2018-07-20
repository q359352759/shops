<template>
	<div id="experience">
		<header class="mui-bar mui-bar-nav">
		    <a class="mui-action-back mui-icon mui-icon-left-nav mui-pull-left"></a>
		    <h1 class="mui-title">内部体验</h1>
		</header>
		<div class="mui-content">
			<form @submit.prevent="form_1()" class="box_1">
					<ul class="">
				    	<li class="header_1">当前可用体验余额：{{maxnumber}}元</li>
				    	<li class="title_1">体验金额：</li>
				    	<li class="money_1">
				    		<input type="number" min="10" :max="maxnumber" v-model="total_fee" required="" placeholder="输入金额" />
				    	</li>
				    	<li class="btn_1">
				    		<button type="submit" class="btn">立即体验</button>
				    	</li>
				   </ul>
		    </form>
		</div>
	</div>
</template>

<script>
	import {http} from '@/assets/fc';
	export default{
		name:'',
		data(){
			return{
				openid:localStorage.openid,
				total_fee:'',				//充值金额
				userId:localStorage.ids,
				maxnumber:0,				//剩余体验金额
			}
		},
		methods:{
			form_1(){
				//Hb,商家Hb充值
				var obj=new Object();
					obj.openid=this.openid 		//用户openid
					obj.chargetype=1			//充值类型 1:hb充值 4:商家HB充值
					obj.total_fee=this.total_fee 			//充值金额
					obj.memberid=this.userId	//用户id
					obj.isexperience=0			//是否是体验金0:是 1:不是 	
//					alert(JSON.stringify(obj));
				http('post','/platform/weixinpay/hblbOrder',obj,this.hblbOrder_return)
			},
			hblbOrder_return(x){
				console.log(x);
				var this_1=this;
				if(x.code==10000){
					wx.chooseWXPay({
						timestamp: x.data.timeStamp, 		// 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
						nonceStr: x.data.nonceStr, // 支付签名随机串，不长于 32 位
						package: x.data.package, // 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=\*\*\*）
						signType: x.data.signType, // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
						paySign: x.data.paySign, // 支付签名
						success: function (res) {
							// 支付成功后的回调函数
//							alert(JSON.stringify(res))
//							if(res.errMsg=='chooseWXPay:ok'){
								//hb充值成功后回调方法
								var obj=new Object();
									obj.userid=this_1.userId;
									obj.orderid= x.data.orderid;
									obj.chargetype=1				 //1 hb充值 4商家hb充值 
//									alert(JSON.stringify(obj))
								http('post','/platform/weixinpay/hbpay_notify',obj,this_1.hbpay_notify)
//							}
						},
					});
//					function onBridgeReady(){
//					   WeixinJSBridge.invoke(
//					       'getBrandWCPayRequest', {
//					           "appId":x.data.appId,     //公众号名称，由商户传入     
//					           "timeStamp":x.data.timeStamp,         //时间戳，自1970年以来的秒数     
//					           "nonceStr":x.data.nonceStr, //随机串     
//					           "package":x.data.package,     
//					           "signType":x.data.signType,         //微信签名方式：     
//					           "paySign":x.data.paySign //微信签名 
//					       	},
//					       	function(res){
//					       		alert(JSON.stringify(res))
//					           	if(res.err_msg == "get_brand_wcpay_request:ok" ) {
//					           		alert('支付完成')
//					           	}     // 使用以上方式判断前端返回,微信团队郑重提示：res.err_msg将在用户支付成功后返回    ok，但并不保证它绝对可靠。
//					       	}
//					   	); 
//					}
//					//判断是否支持微信支付
//					if (typeof WeixinJSBridge == "undefined"){
//					   if( document.addEventListener ){
//					       document.addEventListener('WeixinJSBridgeReady', onBridgeReady, false);
//					   }else if (document.attachEvent){
//					       document.attachEvent('WeixinJSBridgeReady', onBridgeReady); 
//					       document.attachEvent('onWeixinJSBridgeReady', onBridgeReady);
//					   }
//					}else{
//					   onBridgeReady();
//					}
					
				}else{
					mui.toast(x.description,{ duration:2000, type:'div' })
				}
			},
			hbpay_notify(x){
//				alert(JSON.stringify(x));
				this.findExperience();
			},
			//查询当前用户还可以体验的金额
			findExperience(){
				http('post','/hb/hbDistribution/lastExperience',{'memberid':this.userId},this.findExperience_return)
			},
			findExperience_return(x){
				console.log('查询当前用户还可以体验的金额',x)
				this.maxnumber=x
			}
		},
		mounted(){
			this.findExperience();
			
		},
		created(){
			
		}
	}
</script>

<style>
	#experience{
		height: 100%;
	}
	#experience .mui-content{
		height: 100%;
	}
	#experience .box_1{
		height: 100%;
		background-image: url('../assets/img/893645114956839482.jpg');
		background-size:cover;
		padding: 1px 0px;
		display: block;
	}
	#experience .box_1 ul{
		margin: 2.6rem 0.82rem 0px;
		padding: 1px;
		background-color: rgba(255, 255, 255, 0.8);
		border-radius: 20px;
		text-align: center;
	}
	#experience .box_1 .header_1{
		font-size: 0.24rem;
		letter-spacing: 0px;
		color: #b2b2b2;
		padding: 0.45rem 0px 0px;
	}
	#experience .box_1 .title_1{
		font-size: 0.43rem;
		letter-spacing: 0px;
		color: #3fcfc2;
		margin: 1.32rem 0px 0px;
	}
	#experience .box_1 .money_1{
		margin: 0.46rem 0px 0px;
		text-align: center;
	}
	#experience .box_1 .money_1 input{
		width: 2.45rem;
		border: none;
		background: none;
		border-bottom: 1px solid #3fcfc2;
		text-align: center;
		font-size: 0.24rem;
		color: rgba(0, 0, 0, 0.5);
		padding: 0px;
		margin: 0px;
		border-radius: 0px;
		height: auto;
	}
	#experience .box_1 .btn_1{
		margin: 1.7rem 0px 0.38rem;
		text-align: center;
	}
	#experience .box_1 .btn_1 button{
		width: 2rem;
		height: 0.62rem;
		background-color: #18a56a;
		border-radius: 5px;
		border: none;
		font-size: 0.36rem;
		color: #ffffff;
		padding: 0px;
	}
	
	
</style>
