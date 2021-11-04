<template>
 <h1 class='header_intro' ref='header_intro'>Click the Amogus</h1>
 <div class="main_container" @click='musicPlay'>
	<div id="bigamogus" ref="bigamogus" title="The Big Amogus" @click="delayedTwerk($event, 'bigamogus')">

	 <Dollar ref='bill0'/>
	 <Dollar ref='bill1'/>
	 <Dollar ref='bill2'/>
	 <Dollar ref='bill3'/>
	 <Dollar ref='bill4'/>
	 <Dollar ref='bill5'/>
	 <Dollar ref='bill6'/>
	 <Dollar ref='bill7'/>
	 <Dollar ref='bill8'/>
	 <Dollar ref='bill9'/>
	 <Dollar ref='bill10'/>
	 <Dollar ref='bill11'/>
	</div>
	<div class="options_container">
	 <button class='button' @click="speedUpAmogus('yellow')" :disabled="!enoughMoney">
		<div>Speed it up Amogus!</div>
		<div>Cost: {{ amogus_twerk_speedup_cost }}$</div>
	 </button>
	 <br>
	 <p>Twerk Speed: {{ yellowAmongusSpeedDisplay }}</p>
	 <p>Amogus' Ass Thrown In a Circle {{ twerks }} Times</p>
	 <p>Hard Earned Cash: {{ money }}$</p>
	</div>
 </div>
 <audio ref="music" controls autoplay loop hidden>
	<source src="./assets/amongus.mp3" type="audio/mpeg">
 </audio>
 <audio ref="amogusaudio" controls hidden>
	<source src="./assets/amogus.mp3" type="audio/mpeg">
 </audio>
</template>

<script>
import Dollar from './components/DollarEffect.vue'

export default {
 name: 'App',
 components: {
	Dollar
 },
 data() {
	return {
	 twerk_disabled: false,
	 twerk_timeout: null,
	 twerk_queued: false,
	 yellow_amogus_speed: 1.0,
	 amogus_twerk_speedup_cost: 2,
	 amogus_speedup_purchase: 0.8, //how much faster the animations should be each purchase
	 twerks: 1,
	 money: 1,
	 x: 0,
	 y: 0,
	 musicPlaying: false
	}
 },
 methods: {
	musicPlay() {
	 if(!this.musicPlaying){
		this.musicPlaying = true
		this.$refs.music.play();
	 }
	},
	amogusClipPlay(){
	 this.$refs.amogusaudio.play();
	},
	resetAmogus(e, element){
	 this.twerks += 1
	 this.money += Math.floor((Math.random() * 3) + 1);
	 for(let dollar in [0,1,2,3,4,5,6,7,8,9,10,11]){
		let temp = this.$refs['bill' + (parseInt(dollar) + 1)].reset(e.x, e.y)
		if(temp != 'activated'){
		 break
		}
	 }
	 this.$refs[element].style.animation = 'none'
	 this.$refs[element].offsetHeight /* trigger reflow */
	 this.$refs[element].style.animation = null
	},
	delayedTwerk(e, element) {
	 if(!this.twerk_disabled){
		this.twerk_disabled = true
		this.twerk_timeout = setTimeout(() => {
		 this.twerk_disabled = false
		 if(this.twerk_queued){
			this.twerk_queued = false
			this.delayedTwerk(e, element)
		 }
		}, (this.yellow_amogus_speed * 1000))
		this.resetAmogus(e, element)
	 }
	 else{
		this.twerk_queued = true
	 }
	},
	speedUpAmogus(color) {
	 if(this.enoughMoney){
		this.amogusClipPlay()
		this.yellow_amogus_speed *= this.amogus_speedup_purchase
		this.money -= this.amogus_twerk_speedup_cost
		this.amogus_twerk_speedup_cost *= 3
	 }
	}

 },
 computed: {
	yellowAmogusSpeed(){
	 return (this.yellow_amogus_speed + "s")
	},
	yellowAmongusSpeedDisplay(){
	 return ((1/this.yellow_amogus_speed).toFixed(2))
	},
	enoughMoney(){
	 return (this.amogus_twerk_speedup_cost <= this.money)
	}
 }

}
</script>

<style>
html, body {
 height: 100%;
 margin: 0;
}
#app {
 font-family: Avenir, Helvetica, Arial, sans-serif;
 -webkit-font-smoothing: antialiased;
 -moz-osx-font-smoothing: grayscale;
 text-align: center;
 color: #fff;
 margin-top: 0px;
 padding-top: 60px;
 background-image: url('./assets/background.png');
 background-size: cover;
 background-repeat: no-repeat;
 min-height: 100%; 
}
.main_container {
 background-color: rgba(1, 1, 1, 0.35);
 margin: auto;
 width: auto;
 padding: 50px;
 display: inline-block;
 border: 0px;
 border-radius: 25px;
}
.options_container {
 background-color: rgba(1, 1, 1, 0.5);
 margin: auto;
 width: auto;
 border: 0px;
 border-radius: 25px;
 padding: 15px;
 display: inline-block;
 border: 0px;
 border-radius: 25px;
}
#bigamogus {
 background-image: url('./assets/amogus.png');
 width: 384px;
 height: 384px;
 animation: anim v-bind('yellowAmogusSpeed') steps(6) 1;
 border: 20px;
 margin: auto;
 user-select: none;
 -webkit-user-select: none;
 -khtml-user-select: none;
 -webkit-touch-callout: none;
 -moz-user-select: none;
 -o-user-select: none;
}
@keyframes anim {
 to { background-position: -2304px; }
}

.button {
 user-select: none;
 -webkit-user-select: none;
 -khtml-user-select: none;
 -webkit-touch-callout: none;
 -moz-user-select: none;
 -o-user-select: none;
 background-color: goldenrod; 
 border: none;
 color: black;
 padding: 16px 32px;
 text-align: center;
 text-decoration: none;
 display: inline-block;
 font-size: 16px;
 margin: 4px 2px;
 transition-duration: 0.4s;
 cursor: pointer;
 border: 2px solid gold;
}

.button:hover {
 background-color: white;
 color: black;
}

.button:disabled {
 background-color: lightgoldenrodyellow;
 color: darkgray;
 cursor: not-allowed;
}

</style>
