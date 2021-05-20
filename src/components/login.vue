<template>
	<div>
		<!-- 导航栏 -->
		<div class="navBarClass">
			<van-nav-bar title="网易严选">
			  <template #left>
			    <van-icon name="wap-home-o" size="68" @click='gotoHome'/>
			  </template>
			  <template #right>
			    <van-icon name="shopping-cart-o" size="68" />
			  </template>
			</van-nav-bar>
		</div>
		
		<!-- 中间图标 -->
		<div class="logoWrap">
			<img src="//yanxuan.nosdn.127.net/39c5e4583753d4c3cb868a64c2c109ea.png" />
		</div>
		
		<div class="btnWrap">
			<van-button 
				:icon='phone_icon'
				type="primary" 
				color="#DD1A21" 
				block
				style='height: 1.3rem;'
				@click='registerBtnEvent'>注册用户名
			</van-button>
			
			<van-button
				:icon='login_icon'
				type="primary" 
				color="#DD1A21" 
				block
				plain
				style='height: 1.3rem;'
				@click='loginBtnEvent'>邮箱账号登录
			</van-button>
			
			<span @click="otherLoginFn">
				其他登录方式 >
			</span>
		</div>
		
		<!-- 用户名、密码登录 -->
		<div class="loginWrap" v-show="loginWrap_show">
			<input v-model='user_val' id='com' type="text" placeholder="邮箱账号" />
			<input v-model='pass_val' id='com' type="password" placeholder="密码" />
			<input @click="userLoginFn"  type="button" value="登录" />
		</div>
		
		<!-- 用户名 -->
		<div class="loginWrap" v-show="register_show">
			<input v-model='re_user_val' id='com' type="text" placeholder="注册账号" />
			<input v-model='re_pass_val' id='com' type="password" placeholder="注册密码" />
			<input @click="getRegisterValFn" type="button" value="注册"  />
		</div>
	</div>
</template>
<script>
	import login_icon from '../assets/login_icon.png'
	import phone_icon from '../assets/phone_icon.png'
	import { Dialog } from 'vant';
	export default {
		name:'login',
		data(){
			return {
				msg:'登录页面',
				login_icon:login_icon,
				phone_icon:phone_icon,
				// 登录用户名,密码
				user_val:'',
				pass_val:'',
				// 注册用户名,密码
				re_user_val:'',
				re_pass_val:'',
				// 保存多个用户信息
				reArrObj:[],
				// 保存获取到的所有用户信息
				saveUserInfo:null,
				loginWrap_show:false,
				register_show:false
			}
		},
		mounted(){
			let _getRegObj = localStorage.getItem('reArrObj');
			console.log(JSON.parse(_getRegObj));
			this.saveUserInfo = JSON.parse(_getRegObj);
		},
		methods: {
			// 回首页
			gotoHome(){
				this.$router.push('/')
			},
			// 显示登录
			loginBtnEvent(){
				// console.log('已经登录！')
				this.loginWrap_show = true;
				this.register_show = false;
				
			},
			// 显示注册
			registerBtnEvent(){
				// console.log('已经登录！')
				this.register_show = true;
				this.loginWrap_show = false;
				
			},
			// 其他登录方式
			otherLoginFn(){
				this.register_show = false;
				this.loginWrap_show = false;
			},
			// 注册用户名,密码
			getRegisterValFn(){
				// console.log(this.re_user_val,this.re_pass_val)
				// 注册用户名
				let _reObj = {
					u:this.re_user_val,
					p:this.re_pass_val
				}
				// 将对象存入数组
				this.reArrObj.push(_reObj);
				// 保存注册信息
				// localStorage里面存的都是string格式的,要用JSON.stringify方法转格式
				localStorage.setItem('reArrObj', JSON.stringify(this.reArrObj));
				// 重置注册输入框
				this.re_user_val = '';
				this.re_pass_val = '';
			},
			// 用户登录
			userLoginFn(){
				// console.log(this.user_val,this.pass_val);
				// for 循环判断是否已经登录
				for(var i=0; i<this.saveUserInfo.length;i++){
					// console.log(this.saveUserInfo[i]);
					if(this.saveUserInfo[i].u ===this.user_val &&
						this.saveUserInfo[i].p === this.pass_val){
							// console.log('已经成功登录');
							Dialog.alert({
								  title: '登录成功',
								  message: '欢迎回来：'+this.user_val,
								}).then(() => {
								  this.$router.push('/');
								});
							break;
					}
					Dialog.alert({
					  title: '登录失败',
					  message: '请注册用户',
					}).then(() => {
					  // 重置注册输入框
					  	this.user_val = '';
					  	this.pass_val = '';
					});
				}
			}
				
			
		},
	}
</script>
<style scoped>
	.logoWrap {
	    text-align: center;
	    padding-top: 2.13333rem;
	    padding-bottom: 3.09333rem;
	}
	.logoWrap img {
	    width: 3.57333rem;
	    height: 1.2rem;
	}
	
	.btnWrap {
	    padding: 0 .53333rem;
	    margin-bottom: 2.73333rem;
	}
	.btnWrap .van-button--normal {
		margin-bottom: .4rem;
	}
	.btnWrap span {
		text-align: center;
		display: block;
		font-size: .4rem
	}
	.btnWrap[data-v-10d9df09] {
		margin-bottom: 1rem;
	}
	.loginWrap {
		
	}
	.loginWrap input{
		margin: .2rem auto;
		display: block;
		border: 0;
		font-size: .4rem
	}
	#com {
		border-bottom: solid 1px #a8a8a8
	}
</style>