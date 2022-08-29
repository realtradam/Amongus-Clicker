<template>
<header><a href="https://github.com/realtradam/Amongus-Clicker" target="_blank"><div></div><p>View Source Code</p></a></header>
 <h1 class='header_intro' ref='header_intro'>ඞ  Click the Crewmate to Make Him Dance Faster ඞ </h1>
 <h2>For each dance complete, get cash money!</h2>
 <div class="main_container" @click='musicPlay'>
	<div id="bigamogus" ref="bigamogus" title="The Big Amogus" @click="speedUpDance($event)">

	</div>
	<div class="options_container">
	<div>
	<div style="display:flex;flex-direction:row;justify-content:center;gap:5px;font-size: 30px"><div>Dance Speed:</div><b><div>{{ format(round(amogus_speed)) }}</div></b></div>
	 <p>Hard Earned Cash 💸: ${{ format(round(money)) }}</p>
	 <p>Dance Moves Completed: <b>{{ format(amogus_dance_routines) }} Times</b></p>
	 </div>
	<div class="option">
		<button class='button improveclicks' @click="buyMotivate()" :disabled="buyMotivateDisabled">
			<div>Improve Clicks</div>
			<div>Cost: {{ format(round(amogus_motivate_cost)) }}$</div>
		</button>
		<p>
			Click Motivation: {{ format(round(amogus_motivate)) }}
		</p>
	 </div>
	<div class="option">
		<button class='button improvestamina' @click="buyDecay()" :disabled="buyDecayDisabled">
			<div>Improve Stamina</div>
			<div>Cost: {{ format(round(amogus_decay_cost)) }}$</div>
		</button>
		<p>
			Dance Stamina: {{ format(round(amogus_decay)) }}
		</p>
	</div>
	<div class="option">
		<button class='button improveearnings' @click="buyEarnings()" :disabled="buyEarningsDisabled">
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
import { createApp } from 'vue'

export default {
 name: 'App',
 components: {
	Dollar
 },
 data() {
	return {
		amogus_speed: 0,

		amogus_min: 1.0,

		amogus_motivate: 0.1,
		amogus_motivate_cost: 2.0,
		amogus_motivate_cost_inflation: 1.3,
		amogus_motivate_improvement: 0.05,

		amogus_decay: 1,
		amogus_decay_cost: 5.0,
		amogus_decay_cost_inflation: 4.0,
		amogus_decay_improvement: 1.4,
		amogus_decay_modifier: 0.005,

		amogus_earnings: 2.0,
		amogus_earnings_cost: 10.0,
		amogus_earnings_cost_inflation: 5.0,
		amogus_earnings_improvement: 1.3,

		amogus_width: 298,
		amogus_height: 298,
		amogus_frame: 0,
		//amogus_frame_count: 65,
		amogus_frame_row: 0,
		amogus_frame_counts: [18, 24, 21, 24],
		amogus_frame_buffer: 0,
		amogus_dance_routines: 0,
		amogus_first_click: true,
		dollar_visuals: [],
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
			this.amogusClipPlay();
		}
		//amogus_min_cost
		//amogus_decay_cost
	},
	buyDecay(){
		if(!this.buyDecayDisabled){
			this.money -= this.amogus_decay_cost;
			this.amogus_decay_cost *= this.amogus_decay_cost_inflation;
			this.amogus_decay *= this.amogus_decay_improvement;
			this.amogusClipPlay();
		}
	},
	buyEarnings(){
		if(!this.buyEarningsDisabled){
			this.money -= this.amogus_earnings_cost;
			this.amogus_earnings_cost *= this.amogus_earnings_cost_inflation;
			this.amogus_earnings *= this.amogus_earnings_improvement;
			this.amogusClipPlay();
		}
	},
	amogusFrameAdvance(){
		this.amogus_frame += 1;
		while(this.amogus_frame > this.amogus_frame_counts[this.amogus_frame_row]){
			this.money += Math.random() * (this.amogus_earnings * 1.5 - (this.amogus_earnings * 0.5)) + (this.amogus_earnings * 0.5);
			this.amogus_dance_routines += 1;
			this.amogus_frame -= this.amogus_frame_counts[this.amogus_frame_row];
			this.amogus_frame_row = parseInt(Math.random() * 4);
			var among = this.$refs["bigamogus"];
			if(among){
				this.renderDollar(Math.random() * (this.amogus_height - 100) + (among.offsetLeft - 25),
				Math.random() * (this.amogus_width - 100) + (among.offsetTop - 25)
				);
			}
		}
	},
	renderDollar(x, y){
		find: {
		 	for(let dollar in this.dollar_visuals){
				let temp = this.dollar_visuals[dollar].reset(x,y)
				if(temp != 'activated'){
					break find;
				}
			}
			var among = this.$refs["bigamogus"];
			//dollar.$mount();
			const tempelem = document.createElement('div');
			among.appendChild(tempelem);
			var dollar = createApp(Dollar).mount(tempelem);
			//tempelem.appendChild(dollar.$el);
			dollar.reset(x,y);
			this.dollar_visuals.push(dollar);
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
			  }, (1/60) * 1000);
		  },
 computed: {
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
	 return (-(this.amogus_frame * this.amogus_width) + 'px ' + (this.amogus_frame_row * this.amogus_height) + 'px')
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
 padding: 10px;
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
 padding: 20px;
 display: inline-block;
 border: 0px;
 border-radius: 25px;
}
header {
	position: fixed;
	background-color: black;
	border-radius: 10px;
	padding: 10px;
	top: 15px;
	left: 15px;
}
header a {
	display: flex;
	flex-direction: row;
	align-items: center;
	gap: 10px;
	color: white;
	text-decoration: none;
	transition-duration: 0.4s;
}
header a:hover {
	color: lightsteelblue;
	transition-duration: 0.4s;
}
header a div {
	width: 20px;
	height: 20px;
	background-size: 20px;
	background-image: url('./assets/GitHub-Mark-120px-plus.png');
}
.options_container {
 background-color: rgba(1, 1, 1, 0.5);
 margin: auto;
 width: auto;
 border: 0px;
 border-radius: 25px;
 padding: 35px;
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
 cursor: pointer;
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
 border: none;
 color: black;
 padding: 16px 32px;
 text-align: center;
 text-decoration: none;
 display: inline-block;
 font-size: 16px;
 transition-duration: 0.4s;
 cursor: pointer;
 width: 200px;
 flex-grow: 0;
 flex-shrink: 0;
}

.button:hover {
 background-color: white;
 color: black;
}

.improveclicks {
 background-color: goldenrod; 
 border: 2px solid gold;
}
.improveclicks:disabled {
 background-color: lightgoldenrodyellow;
 border: 2px solid gold;
}

.improvestamina {
 background-color: dodgerblue; 
 border: 2px solid cyan;
}
.improvestamina:disabled {
 background-color: lightcyan;
 border: 2px solid cyan;
}

.improveearnings {
 background-color: limegreen; 
 border: 2px solid chartreuse;
}
.improveearnings:disabled {
 background-color: #bbeebb;
 border: 2px solid chartreuse;
}

.button:disabled {
 color: darkgray;
 cursor: not-allowed;
}

</style>
