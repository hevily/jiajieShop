<template>
	<div class="squared">
		<div>
			<van-nav-bar title="发布需求" left-arrow @click-left="onClickLeft" @click-right="onClickRight">
				<van-icon name="wap-nav" color="white" slot="right" />
			</van-nav-bar>
			<div v-show="rishow" @touchmove.prevent="orishow()" @click.stop="orishow()" style="position: fixed;z-index: 9999; top: 0;right: 0;left: 0;bottom: 0; background: rgba(0,0,0,0); ">
				<div style="position: absolute;z-index: 9999; top: .45rem;right: 0;width: 1rem ;background: rgba(0,0,0,.2); ">
					<head-ri></head-ri>
				</div>
			</div>
		</div>
		<ul class="sq_ul">

			<li v-for="(item,index) in list" class="sq_ul_li" @click="sq_li(item,index)">
				<div>
					<!--<img :src="uploadFileUrl+ item.cate_icon" />-->
					<i :class="`iconfont ${item.cate_icon}`"></i>
				</div>
				<div>
					{{item.cat_name}}

				</div>
			</li>
		</ul>

	</div>
</template>

<script>
	import api from '@/api/api'
	import headRi from '@/page/pages/head_ri_sub'
	export default {
		components: {
			headRi
		},
		data() {
			return {
				uploadFileUrl: api.uploadFileUrl + '/',
				rishow: false,
				list: []
			}
		},
		mounted() { //生命周期
			this.list_posh()
		},
		methods: { //方法
			onClickLeft() {
				let that = this
				that.$router.push({
					path: 'home',

				})
			},
			onClickRight() {
				let that = this
				that.rishow = !that.rishow
			},
			orishow() {
				let that = this
				that.rishow = false
			},
			sq_li(item, index) {
				this.$fetch('category_list', {'data-set': 'tree'}, '-' + item.id).then(rs =>{
          if(rs.length < 1){
            this.$router.push({
              path: 'release_dem_rele',
              query: {
                level_1: this.$route.query.ids,
                level_2: this.$route.query.ids,
                level_3: this.$route.query.ids
              }
            })
          }else{
            this.$router.push({
              path: 'release_dem_category',
              query: {
                ids: item.id
              }
            })
          }
				})
			},
			list_posh() {
				let that = this
				let lists = {};
				lists.condition = {
					parent_id: 0
				};
				this.$fetch('category_list', lists).then(rs =>{
          that.list = rs
        })
			},

		},
	}
</script>

<style scoped lang="less">
@import "../../assets/categoryIcon/iconfont.css";
</style>
<style scoped>
	.sq_ul {
		margin: .1rem 0 0 0;
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		flex-wrap: wrap;
	}

	.sq_ul_li {
		text-align: center;
		flex: 0 0 33.33%;
		margin: .1rem 0;
	}

	.sq_ul_li>div:nth-child(1) {
		margin-bottom: .05rem;
	}

	.sq_ul_li div img {
		width: .55rem;
	}
</style>
<style>
	.squared .van-nav-bar {
		background: #18b4ed !important;
	}

	.squared .van-nav-bar__title {
		color: #fff !important;
	}

	.squared .van-icon {
		color: White !important;
	}
</style>
