<template>
	<section id="wrapper">
		<div class="indentation">
			<button v-on:click="addFiveMinutes">Плюс</button>
			<button @click="subtractFiveMinutes">Минус</button>
		</div>
		<div class="timer indentation"><p class="text">{{ hours }}:{{ minutes }}:{{ seconds }} </p></div>
		<div class="start indentation">
			<a href="#">
				<span class="start-timer" @click="addToBase">Начать</span>
			</a>
		</div>
	</section>
</template>

<script>
export default {
  data() {
	
    return {
		dateNow: new Date(),
		hours: null,
		minutes: null,
		seconds: null,
		/*hours: '00',
		minutes: '10',
		seconds: '00',*/
		base: []
    }
  },
  mounted() {
		this.dateNow.setHours(0, 10, 0);
		this.updateTime();
	},

  methods: {
	updateTime() {
		this.hours = this.dateNow.getHours() < 10 ? "0" + this.dateNow.getHours() : this.dateNow.getHours();
		this.minutes = this.dateNow.getMinutes() < 10 ? "0" + this.dateNow.getMinutes() : this.dateNow.getMinutes();
		this.seconds = this.dateNow.getSeconds() < 10 ? "0" + this.dateNow.getSeconds() : this.dateNow.getSeconds();
	},
    addFiveMinutes() {
		if(this.dateNow.getHours() <= 1) {
			this.dateNow.setMinutes(this.dateNow.getMinutes() + 5)
			this.updateTime();
		}
    },
    subtractFiveMinutes() {
		if(this.dateNow.getMinutes() > 10 && this.dateNow.getHours() == 0 || this.dateNow.getHours() == 1 || this.dateNow.getHours() == 2) {
			this.dateNow.setMinutes(this.dateNow.getMinutes() - 5)
			this.updateTime();
		}
    },
	addToBase() {
		let time = {
			hour: this.hours,
			minute: this.minutes,
			second: this.seconds
		};
		this.base.push(time);
		fetch('http://127.0.0.1:8000',{
			method: "GET", // *GET, POST, PUT, DELETE, etc.
			//mode: "no-cors", // no-cors, *cors, same-origin

			headers: {
			"accept": "application/json",
			"content-type": "application/json",
			// 'Content-Type': 'application/x-www-form-urlencoded',
			},
		}
		)
			.then(response => response.json())
			.then(data => console.log(data))
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
