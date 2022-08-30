<template>
	<img v-show='activated' src="../assets/bill.png" class="bill" ref="bill">
</template>

<script>
	export default {
		data() {
			return {
				x: 0,
				y: 0,
				startx: 0,
				starty: 0,
				targetx: 100,
				targety: 100,
				activated: false
			}
		},
		methods: {
			reset(x,y) {
				if(this.activated){
					return 'activated'
				}
				this.x = parseInt(x)
				this.y = parseInt(y)
				this.startx = x + 'px'
				this.starty = y + 'px'
				this.targetx = (this.x + Math.floor(Math.random() * 100) - 50) + 'px'
				this.targety = (this.y + Math.floor(Math.random() * 100) - 50) + 'px'
				this.activated = true
				this.timeout = setTimeout(() => {
					this.activated = false
				}, (1.2 * 1000))
			},
		}
	}
</script>

<style scoped>
	img {
		user-select: none;
		-webkit-user-select: none;
		-khtml-user-select: none;
		-webkit-touch-callout: none;
		-moz-user-select: none;
		-o-user-select: none;
		width: 100px;
		height: 100px;
		animation-name: bill;
		animation-duration: 1.3s;
		animation-iteration-count: infinite;
		position: absolute
	}

	@keyframes bill {
		0%   {opacity: 0; left: v-bind('startx'); top: v-bind('starty');}
		1%   {opacity: 1; left: v-bind('startx'); top: v-bind('starty');}
		100% {opacity: 0; left: v-bind('targetx'); top: v-bind('targety');}
	}
</style>
