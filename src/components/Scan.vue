<template>
	<div id="Scan">
		<!--<div @click="saoyisao()">扫一扫</div>-->
	</div>
</template>

<script>
	export default{
		components:{
		},
		data:function(){
			return{}
		},
		methods:{
//			saoyisao(){
//				
//			}
		},
		mounted:function(){
			wx.ready(function(){
				wx.scanQRCode({
	   				needResult: 0, // 默认为0，扫描结果由微信处理，1则直接返回扫描结果，
	    			scanType: ["qrCode","barCode"], // 可以指定扫二维码还是一维码，默认二者都有
	    			success: function (res) {
	//  				alert(JSON.stringify(res))
					}
				});
			})
			
		},
		created:function(){
			
		}
	}
</script>
<style>
</style>