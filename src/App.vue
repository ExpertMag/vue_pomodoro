<template>
	<section id="wrapper">
		<div class="indentation">
			<button v-on:click="addFiveMinutes">Плюс</button>
			<button @click="subtractFiveMinutes">Минус</button>
			<button @click="getFullHours">ncn</button>
		</div>
		<div class="timer indentation"><p class="text">{{ hours }}:{{ minutes }}:{{ seconds }} </p></div>
		<div class="start indentation">
			<a href="#">
				<span class="start-timer">Начать</span>
			</a>
		</div>
	</section>
</template>

<script>
export default {
  data() {

	
	//const minutes = now.getMinutes();
	//const seconds = now.getSeconds();
	//const totalTime = (hours * 3600 + minutes * 60 + seconds) * 1000;

	//const hours2 = Math.floor(totalTime / 3600000);
	//const minutes2 = Math.floor((totalTime % 3600000) / 60000);
	//const seconds2 = Math.floor((totalTime % 60000) / 1000);

	//const all = (`${hours}`)
    return {
		hours: '00',
		minutes: '10',
		seconds: '00'
    }
  },
  methods: {
    addFiveMinutes() {
		if(Number(this.minutes) == 50) {
			if(Number(this.hours) < 10) {
				this.hours = '0' + ((Number(this.hours) + 1).toString());
				this.minutes = '00';
				if(this.hours == '010') {
					this.hours = '10';
				}
			} else {
				this.hours = (Number(this.hours) + 1).toString();
				this.minutes = '00';
			}

		} else {
			this.minutes = (Number(this.minutes) + 10).toString();
		}

    },
    subtractFiveMinutes() {
		/*if(Number(this.minutes) > 10) {
			
		}*/
		this.minutes = (Number(this.minutes) - 5).toString();
    },
	getFullHours(){
		const now = 3700000;
		this.hours = now / 3600000
		if(this.hours <= 1) {
			return this.hours = '00'
		} else {
			return this.hours = 10
		}
		
	}
  }
}
</script>

<style>
body {
	background: linear-gradient(180deg, rgb(32, 38, 57) 11.4%, rgb(63, 76, 119) 70.2%);
	color: #fff;

}

/*весь блок*/
#wrapper {
	display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
    margin: 0;
}

.indentation {
	padding: 20px 5px;
}

/*таймер*/
.timer {
    text-transform: uppercase;
    font-size: 24px;
}
.text {
	margin: 0px;
}


/*кнопка*/
.start{
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
}

.start a{
	position: relative;
	width: 160px;
	height: 60px;
	display: inline-block;
	background: #fff;
}

.start a:before,
.start a:after
{
	content:'';
	position: absolute;
	inset: 0;
	transition: 0.5s;
	background: #f00
}
.start a:before,
.start a:after
{
	background: linear-gradient(45deg, #f0075b, #0e1538, #1aff22);
}
.start a:hover:before
{
	inset: -3px;  
}
.start a:hover:after
{
	inset: -3px;  
	filter: blur(10px);
}
.start a span{
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	display: inline-block;
	background: #0e1538;
	z-index: 10;
	display: flex;
	justify-content: center;
	align-items: center;
	font-size: 1.2em;
	text-transform: uppercase;
	letter-spacing: 2px;
	color: #fff;
	border: 1px solid #040a29;
	overflow: hidden;
}
.start a span::before{
	content:'';
	position: absolute;
	top: 0;
	left: -50%;
	width: 100%;
	height: 100%;
	background: rgba(255,255,255,0.075);
	transform: skew(25deg)
}
</style>
