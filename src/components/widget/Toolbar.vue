<template>
	<div class="toolbar noselect">
		<img class="flower" src="../../assets/flower.png">
		<div class="container">
			<img class="logo" src="../../assets/logo.png">
			<div class="tab ripple" :class="curTab === index ? 'active' : ''" :style="'width:' + tabWidth + '%'"
			v-for="(item, index) of rankList[curRank].sub[curSubRank].list" @click="select(index)">
			{{ item.name }}</div>
			<div class="tab right ripple" @click="openSelector()" :style="'width:' + tabWidth + '%'">
				<span class="more"></span>
				<span class="more"></span>
				<span class="more"></span>
			</div>
			<span class="indicator" :style="'left:' + tabWidth * curTab + '%;width:' + tabWidth + '%'"></span>
		</div>
	</div>
</template>

<script>
	import { mapActions, mapGetters } from 'vuex'
	export default {

		name: 'Toolbar',

		data () {
			return {
			};
		},
		created() {
			this.updateTabRank();
		},
		computed: {
			...mapGetters({
				rankList: 'getRankList',
				curRank: 'getCurRank',
				curSubRank: 'getCurSubRank',
				curTab: 'getCurTab',
				curSubRankData: 'getCurSubRankData',
				curRankData: 'getCurRankData'
			}),
			tabWidth() {
				return 100 / (this.curSubRankData.list.length + 1);
			}
		},
		methods: {
			...mapActions({
				showSelector: 'showSelector',
				setCurSubRank: 'setCurSubRank',
				setCurTab: 'setCurTab',
				updateTabRank: 'updateTabRank'
			}),
			select(index) {
				this.setCurTab(index);
				this.updateTabRank();
			},

			openSelector() {
				this.showSelector({data: this.curRankData.sub.map(item => {
					return item.name;
				}), callback: index => {
					this.setCurSubRank(index);
					this.updateTabRank();
				}});
			}
		}
	};
</script>

<style lang="scss" scoped>
	@import "~style/_responsive";
	@import "~style/_variables";
	.toolbar {
		position: relative;
		background: $accent_color;
		height: 3 + cr($target: 1px); //box-sizing:boder-box原因, 加上border-top的偏差
		margin-top: 3rem;
		border-top: 1px solid #b889cb;
		>.flower {
			z-index: 40000;
			display: inline-block;
			position: absolute;
			top: 0;
			left: 0;
			transform: translate(0%, -50%);
			height: 6rem;
			pointer-events: none;
		}
		>.container {
			position: relative;
			&:after {
				clear: both;
				content: "";
				display: block;
			}
			@include responsive($breakpoint-xs) {
				padding: 0;
			} 
			>.logo {
				display: inline-block;
				position: absolute;
				left: 50%;
				padding-left: 0.91/54 * 499 * 0.4 * 1rem;
				height: cr($target: 30px);
				top: 0;
				transform: translate(-50%, -100%);
				margin-top: cr($target: 2px);
				z-index: 20000;
			}
			>.tab {
				display: inline-block;
				float: left;
				color: white;
				cursor: pointer;
				// width: 1 / 5 * 100%;
				font-size: cr($target: 14px);
				text-align: center;
				vertical-align: middle;
				line-height: 3rem;
				>.more {
					text-align: center;
					vertical-align: middle;
					display: inline-block;
					background: white;
					width: cr($target: 5px);
					height: cr($target: 5px);
					border-radius: 50%;
				}
				&:after {
					content: '';
					display: block;

				}
				&.active {
					color: white;
				}
				&.right {
					float: right;
				}
			}
			>.indicator {
				// width: 1/5 * 100%;
				background: white;
				height: cr($target: 3px);
				position: absolute;
				bottom: 0;
				transition-property: left;
				transition-duration: 0.3s;
				transition-timing-function: ease;
			}
		}
	}
</style>