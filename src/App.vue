<template>
 <h1 class='header_intro' ref='header_intro'>Click the Amogus</h1>
 <div class="main_container" @click='musicPlay'>
	<div id="bigamogus" ref="bigamogus" title="The Big Amogus" @click="speedUpDance($event)">

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
	 <button class='button' @click="speedUpAmogus()" :disabled="!enoughMoney">
		<div>Speed it up Amogus!</div>
		<div>Cost: {{ 0 }}$</div>
	 </button>
	 <button class='button' @click="speedUpAmogus()" :disabled="!enoughMoney">
		<div>Speed it up Amogus!</div>
		<div>Cost: {{ 0 }}$</div>
	 </button>
	 <br>
	 <p>Dance Speed: {{ amogusSpeedRounded }}</p>
	 <p>Mouse Motivation Power: {{ amogus_motivate }}</p>
	 <p>Motivator: {{ amogus_motivate }}</p>
	 <p>Dance Moves Completed: <b>{{ amogus_dance_routines }} Times</b></p>
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
		amogus_min: 1.0,
		amogus_max: 2.0,
		amogus_speed: 0.0,
		amogus_decay: 0.995,
		amogus_motivate: 0.10,
		amogus_width: 298,
		amogus_height: 298,
		amogus_frame: 0,
		amogus_frame_count: 65,
		amogus_frame_buffer: 0,
		amogus_dance_routines: 0,
		money: 1,
		x: 0,
		y: 0,
		musicPlaying: false
	}
 },
 methods: {
	musicPlay() {
	 if(!this.musicPlaying){
		this.musicPlaying = true;
		this.$refs.music.play();
	 }
	},
	amogusClipPlay(){
	 this.$refs.amogusaudio.play();
	},
	speedUpDance(e){
		this.amogus_speed += this.amogus_motivate;
	},
	amogusFrameAdvance(){
		this.amogus_frame += 1;
		while(this.amogus_frame > this.amogus_frame_count){
			this.amogus_dance_routines += 1;
			this.amogus_frame -= this.amogus_frame_count;
	 	for(let dollar in [0,1,2,3,4,5,6,7,8,9,10,11]){
			var among = this.$refs["bigamogus"];
			let temp = this.$refs['bill' + (parseInt(dollar) + 1)].reset(
				Math.random() * (this.amogus_height - 100) + (among.offsetLeft - 25),
				Math.random() * (this.amogus_width - 100) + (among.offsetTop - 25)
				)
			if(temp != 'activated'){
			 break
			}
		}
		}
	},
	amogusResolveBuffer(){
	 while(this.amogus_frame_buffer >= 1.0){
		this.amogus_frame_buffer -= 1.0;
		this.amogusFrameAdvance();
	 }
	}
	/*
	resetAmogus(e, element){
	 this.twerks += 1
	 this.money += Math.floor((Math.random() * 3) + 1);
	 for(let dollar in [0,1,2,3,4,5,6,7,8,9,10,11]){
		let temp = this.$refs['bill' + (parseInt(dollar) + 1)].reset(e.x, e.y)
		if(temp != 'activated'){
		 break
		}
	 }*/
	 //this.$refs[element].style.animation = 'none'
	 //this.$refs[element].offsetHeight /* trigger reflow */
	 //this.$refs[element].style.animation = null
	},
		  mounted(){
			var vm = this;
			this.interval = setInterval(function () {
			  if(vm.amogus_speed > vm.amogus_min) {
				vm.amogus_speed *= vm.amogus_decay
			  }
			  vm.amogus_frame_buffer += (vm.amogus_speed * 0.1);
				vm.amogusResolveBuffer();
				//console.log("buffr: " + vm.amogus_frame_buffer + "\nspeed: " + vm.amogus_speed);
			  }, (1/60) * 1000);
		  },
 computed: {
	amogusSpeedSeconds(){
	 return (this.amogusSpeedRounded + "s")
	},
	amogusSpeedRounded(){
	 return ((this.amogus_speed).toFixed(2))
	},
	enoughMoney(){
	 return (this.amogus_twerk_speedup_cost <= this.money)
	},
	amogusFramePosition(){
	 return ((this.amogus_frame * this.amogus_width) + 'px 0px')
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
 background: url('./assets/amongusdance.png') v-bind('amogusFramePosition');
 width: 298px; 
 height: 298px;
 border: 0px;
 margin: auto;
 user-select: none;
 -webkit-user-select: none;
 -khtml-user-select: none;
 -webkit-touch-callout: none;
 -moz-user-select: none;
 -o-user-select: none;
}
@keyframes anim {
 to { background-position: -19370px; }
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
