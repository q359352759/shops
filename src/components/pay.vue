<template>

		<footer id="pay">
			<ul>
				<li>
					<router-link to="/collect">
						<span>
							<img src="@/assets/img/money_03.jpg" />
						</span>
						<i>收钱</i>
					</router-link>
				</li>
				<li>
					<!--<router-link to="/Scan">-->
					<a @click="saoyisao()">
						<span>
							<img src="@/assets/img/money_05.jpg" />
						</span>
						<i>扫一扫</i>
					</a>
					<!--</router-link>-->
				</li>
				<li>
					<!--<router-link to="/recriving">-->
					<a @click="Shut_down()">
						<span>
							<img src="@/assets/img/money_07.jpg" />
						</span>
						<i>付款</i>
					</a>
					<!--</router-link>-->
				</li>

			</ul>
		</footer>
</template>

<script>
	export default{
		name:'',
		data(){
			return{}
		},
		methods:{
			Shut_down(){
				console.log(121)
				mui.toast('暂未开放',{ duration:2000, type:'div' });
			},
			saoyisao(){
				wx.scanQRCode({
   					needResult: 0, // 默认为0，扫描结果由微信处理，1则直接返回扫描结果，
    				scanType: ["qrCode","barCode"], // 可以指定扫二维码还是一维码，默认二者都有
    				success: function (res) {
//  					alert(JSON.stringify(res))
					}
				});
			}
		},
		mounted(){
			
		},
		
	}
</script>

<style>
	#pay {
		position: fixed;
		left: 0px;
		bottom: 0px;
		margin: 0 0.34rem;
		width: 100%;
		padding: 0.44rem 0px;
		background: #00a2eb;
	}
	
	#pay ul {
		width: 100;
	}
	
	#pay ul li {
		float: left;
		width: calc((100% - 0.68rem) / 3);
		text-align: center;
	}
	
	#pay ul li img {
		width: 0.4rem;
		height: 0.4rem;
	}
	
	#pay ul li i {
		color: white;
		font-size: 0.18rem;
	}
	
	#pay ul li span {
		display: block;
	}
	#pay footer{position: absolute;left:0;bottom:0.44rem;margin:0 0.34rem;width:100%;}
	#pay footer ul{width:100;}
	#pay footer ul li{float: left;width:calc((100% - 0.68rem) / 3);text-align: center;}
	#pay footer ul li img{width:0.4rem;height:0.4rem;}
	#pay footer ul li i{color:white;font-size:0.18rem;}
	#pay footer ul li span{display: block;}
</style>