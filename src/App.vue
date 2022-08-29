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
	 <Dollar ref='bill12'/>
	 <Dollar ref='bill13'/>
	 <Dollar ref='bill14'/>
	 <Dollar ref='bill15'/>
	</div>
	<div class="options_container">
	<div>
	<div style="display:flex;flex-direction:row;justify-content:center;gap:5px;font-size: 30px"><div>Dance Speed:</div><b><div>{{ format(amogusSpeedRounded) }}</div></b></div>
	 <p>Hard Earned Cash 💸: ${{ format(round(money)) }}</p>
	 <p>Dance Moves Completed: <b>{{ format(amogus_dance_routines) }} Times</b></p>
	 </div>
	<div class="option">
		<button class='button' @click="buyMotivate()" :disabled="buyMotivateDisabled">
			<div>Improve Clicks</div>
			<div>Cost: {{ format(round(amogus_motivate_cost)) }}$</div>
		</button>
		<p>
			Click Motivation: {{ format(round(amogus_motivate)) }}
		</p>
	 </div>
	<div class="option">
		<button class='button' @click="buyDecay()" :disabled="buyDecayDisabled">
			<div>Improve Stamina</div>
			<div>Cost: {{ format(round(amogus_decay_cost)) }}$</div>
		</button>
		<p>
			Dance Stamina: {{ format(round(amogus_decay)) }}
		</p>
	</div>
	<div class="option">
		<button class='button' @click="buyEarnings()" :disabled="buyEarningsDisabled">
			<div>Improve Earnings</div>
			<div>Cost: {{ format(round(amogus_earnings_cost)) }}$</div>
		</button>
		<p>
			Average Earnings: ${{ format(round(amogus_earnings)) }}
		</p>
	 </div>
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
		amogus_speed: 0.0,

		amogus_min: 1.0,

		amogus_motivate: 0.1,
		amogus_motivate_cost: 2.0,
		amogus_motivate_cost_inflation: 1.5,
		amogus_motivate_improvement: 0.05,

		amogus_decay: 1,
		amogus_decay_cost: 5.0,
		amogus_decay_cost_inflation: 4.0,
		amogus_decay_improvement: 1.6,
		amogus_decay_modifier: 0.005,

		amogus_earnings: 5,
		amogus_earnings_cost: 10.0,
		amogus_earnings_cost_inflation: 5.0,
		amogus_earnings_improvement: 1.3,

		amogus_width: 298,
		amogus_height: 298,
		amogus_frame: 0,
		amogus_frame_count: 65,
		amogus_frame_buffer: 0,
		amogus_dance_routines: 0,
		amogus_first_click: true,
		money: 0,
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
		if(this.amogus_first_click){
			this.amogus_speed += 1;
			this.amogus_first_click = false;
		}
	},
	buyMotivate(){
		if(!this.buyMotivateDisabled){
			this.money -= this.amogus_motivate_cost;
			this.amogus_motivate_cost *= this.amogus_motivate_cost_inflation;
			this.amogus_motivate += this.amogus_motivate_improvement;
		}
		//amogus_min_cost
		//amogus_decay_cost
	},
	buyDecay(){
		if(!this.buyDecayDisabled){
			this.money -= this.amogus_decay_cost;
			this.amogus_decay_cost *= this.amogus_decay_cost_inflation;
			this.amogus_decay *= this.amogus_decay_improvement;
		}
	},
	buyEarnings(){
		if(!this.buyEarningsDisabled){
			this.money -= this.amogus_earnings_cost;
			this.amogus_earnings_cost *= this.amogus_earnings_cost_inflation;
			this.amogus_earnings *= this.amogus_earnings_improvement;
		}
	},
	amogusFrameAdvance(){
		this.amogus_frame += 1;
		while(this.amogus_frame > this.amogus_frame_count){
			this.money += Math.random() * (this.amogus_earnings * 1.5 - (this.amogus_earnings * 0.5)) + (this.amogus_earnings * 0.5);
			this.amogus_dance_routines += 1;
			this.amogus_frame -= this.amogus_frame_count;
			var among = this.$refs["bigamogus"];
			if(among){
				this.renderDollar(Math.random() * (this.amogus_height - 100) + (among.offsetLeft - 25),
				Math.random() * (this.amogus_width - 100) + (among.offsetTop - 25)
				);
			}
		}
	},
	renderDollar(x, y){
	 	for(let dollar in [0,1,2,3,4,5,6,7,8,9,10,11]){
			let temp = this.$refs['bill' + (parseInt(dollar) + 1)].reset(x,y)
			if(temp != 'activated'){
			 break
			}
		}
	},
	amogusResolveBuffer(){
	 while(this.amogus_frame_buffer >= 1.0){
		this.amogus_frame_buffer -= 1.0;
		this.amogusFrameAdvance();
	 }
	 },
	 round(num){
		return num.toFixed(2);
	 },
	 format(x){ // from stack overflow
    	if(isNaN(x))return "";

    	var n = x.toString().split('.');
    	return n[0].replace(/\B(?=(\d{3})+(?!\d))/g, ",")+(n.length>1?"."+n[1]:"");

	 },
	},
		  mounted(){
			var vm = this;
			this.interval = setInterval(function () {
			  if(vm.amogus_speed > vm.amogus_min) {
				vm.amogus_speed *= vm.amogus_decay / (vm.amogus_decay + vm.amogus_decay_modifier)
			  }
			  vm.amogus_frame_buffer += (vm.amogus_speed * 0.1);
				vm.amogusResolveBuffer();
				//console.log("buffr: " + vm.amogus_frame_buffer + "\nspeed: " + vm.amogus_speed);
			  }, (1/60) * 1000);
		  },
 computed: {
	amogusSpeedRounded(){
	 return ((this.amogus_speed).toFixed(2))
	},
	buyMotivateDisabled(){
	 return (this.money < this.amogus_motivate_cost)
	},
	buyDecayDisabled(){
	 return (this.money < this.amogus_decay_cost)
	},
	buyEarningsDisabled(){
	 return (this.money < this.amogus_earnings_cost)
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
 font-family: Arial, Helvetica, sans-serif;
}
p {
 margin: 0px;
}
#app {
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
 padding: 30px;
 display: flex;
 flex-direction: column;
 gap: 30px;
 border: 0px;
 border-radius: 25px;
}
.option {
	display: flex;
	flex-direction: row;
	align-items: center;
	gap: 20px;
	margin: 0px;
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
 transition-duration: 0.4s;
 cursor: pointer;
 border: 2px solid gold;
 width: 250px;
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
