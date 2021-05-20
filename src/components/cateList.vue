<template>
	<div style='position: relative;padding-left:2.16rem;'>
		<!-- 侧边栏 -->
		<div class="sidebarClass">
			<van-sidebar v-model="sidebarKey" @change='onChangeSidebar'>
			  <van-sidebar-item 
			   v-for='(n,inx) in sidebar_arr'
			   :key=inx
			   :title='n.txt'/>
			</van-sidebar>
		</div>
		<!-- 右侧商品区域 -->
		<div class="sidebarGoods">
			<div class="goodsItem"
			v-for="(n,inx) in goodsListObj"
			:key=inx
			@click="gotoGoodsBuy(n)">
			<img :src="n.goods_img" />
			<p>{{n.goods_name}}</p>
			<b>{{n.goods_price}}</b>
			</div>
		</div>
		
		<footer_bar></footer_bar>
	</div>
</template>
<script>
	import axios from 'axios'
	import footer_bar from './footer_bar'
	export default {
		name:'cateList',
		data(){
			return {
				msg:'栏目页面组件',
				 sidebarKey: 0,
				 sidebar_arr:[
					 {
					 	txt:'推荐专区'
					 },{
					 	txt:'爆品专区'
					 },{
					     txt:'新品专区'
					 }
				 ],
				 goodsListObj:null
			}
		},
		components: {
			footer_bar
		},
		created(){
			axios.get('http://localhost:3333/cate_goods_list_0')
			        .then(_d=>{
			            console.log(_d.data)
			            this.goodsListObj = _d.data;
			        }) 
		},
		methods: {
			onChangeSidebar(_inx) {
				// console.log(_inx)
				axios.get('http://localhost:3333/cate_goods_list_'+ _inx)
					.then(_d => {
						// console.log(_d.data)
						this.goodsListObj = _d.data;
					})
			},
			gotoGoodsBuy(_n){
				console.log(_n)
				// 带参数的路由跳转
				this.$router.push({
					name:'goods_detail',
					params:{goodsInfo:_n}
				})
			}
		},
	}
</script>
<style scoped>
.sidebarClass{
	position: fixed;left: 0;top: 0;
}

.goodsItem{
	float: left;width: 44%;
	margin: 0rem 0.2rem 0.44rem 0.2rem;
	font-size: .37333rem;overflow: hidden;
}

.goodsItem img{height: 4.6rem;}

</style>