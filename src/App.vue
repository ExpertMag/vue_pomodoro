<template>
	<section id="wrapper">
		<div>
			<div class="indentation">
				<p>{{ textIsWork }}</p>
			</div>
			<div class="indentation">
				<button @click="subtractMinutes">Минус</button>
				<button v-on:click="addMinutes">Плюс</button>
			</div>
			<div class="timer indentation"><p class="text">{{ hours }}:{{ minutes }}:{{ seconds }} </p></div>
			<div class="start indentation">
				<button @click="startTimer" :disabled=disabled v-show="isVisible">Старт</button>
				<button @click="pauseTimer" :disabled=!disabled v-show="!isVisible">Пауза</button>
				<button @click="resetTimer" v-show="!isVisibleReset">Сбросить таймер</button>
				<!--<span class="start-timer" @click="startTimer" :disabled="disabled == 1 ? true : false">Начать</span>-->
			</div>
		</div>
		<FormDescription v-if="this.isWork == false" />
	</section>
</template>

<script>
import FormDescription from '@/components/FormDescription.vue';

export default {
	components: {
		FormDescription
	},
	data() {
		return {
			dateNow: new Date(),
			hours: null,
			minutes: null,
			seconds: null,
			base: [],
			isWork: true,
			textIsWork: 'Работа',
			disabled: false,
			isVisible: true,
			isVisibleReset: true,
			counterId: null,
			buttonText: null,
		}
	},
	mounted() {
		this.dateNow.setHours(0, 0, 5);
		this.updateTime();
		this.buttonText = document.querySelector('.start.indentation button')
	},
	methods: {
		updateTime() {
		this.hours = this.dateNow.getHours() < 10 ? "0" + this.dateNow.getHours() : this.dateNow.getHours();
		this.minutes = this.dateNow.getMinutes() < 10 ? "0" + this.dateNow.getMinutes() : this.dateNow.getMinutes();
		this.seconds = this.dateNow.getSeconds() < 10 ? "0" + this.dateNow.getSeconds() : this.dateNow.getSeconds();
		},
		addMinutes() {
			if(this.isWork == true) {
				if(this.dateNow.getHours() <= 1) {
				this.dateNow.setMinutes(this.dateNow.getMinutes() + 5)
				this.updateTime();
				}
			} else {
				if(this.dateNow.getHours() <= 1) {
				this.dateNow.setMinutes(this.dateNow.getMinutes() + 1)
				this.updateTime();
				}
			}
		},
		subtractMinutes() {
			if(this.isWork == true) {
				if(this.dateNow.getMinutes() > 10 && this.dateNow.getHours() == 0 || this.dateNow.getHours() == 1 || this.dateNow.getHours() == 2) {
				this.dateNow.setMinutes(this.dateNow.getMinutes() - 5)
				this.updateTime();
				}
			} else {
				if(this.dateNow.getMinutes() > 5 && this.dateNow.getHours() == 0 || this.dateNow.getHours() == 1 || this.dateNow.getHours() == 2) {
				this.dateNow.setMinutes(this.dateNow.getMinutes() - 1)
				this.updateTime();
				}
			}
		},
		startTimer() {
			this.toggleVisibility();
			this.isVisibleReset = false;
			if(this.isWork == true) {
				this.textIsWork = 'Работа'
			} else {
				this.textIsWork = 'Отдых'
			}
			let time = {
				id: this.base.length,
				hour: this.hours,
				minute: this.minutes,
				second: this.seconds
			};
			this.base.push(time);

			
			const minusTimer = () => {
				this.dateNow.setSeconds(this.dateNow.getSeconds() - 1);
				this.updateTime();
				if(this.hours == 0 && this.minutes == 0 && this.seconds == 0) {
					clearInterval(this.counterId);
					this.isWork = !this.isWork;
					this.resetTimer();
					this.textIsWork = 'Отдых';
					this.isWork == false ? this.textIsWork = 'Отдых' : this.textIsWork = 'Работа';
				}
			}
			this.counterId = setInterval(minusTimer, 1000);
			
			if(this.textIsWork == 'Отдых') {
1
			}

			/*fetch('http://127.0.0.1:8000',{
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
				.then(data => console.log(data))*/
		},
		toggleVisibility() {
			this.isVisible = !this.isVisible;
			this.disabled = !this.disabled;
		},
		pauseTimer() {
			clearInterval(this.counterId);
			this.isVisible = !this.isVisible;
			this.disabled = !this.disabled;
			this.buttonText.innerText = 'Продолжить';
		},
		resetTimer() {
			clearInterval(this.counterId);
			if(this.isWork == true) {
				this.dateNow.setHours(0, 10, 0);
			} else {
				this.dateNow.setHours(0, 0, 6);
			}
			this.updateTime();
			this.isVisibleReset = true;
			this.buttonText.innerText = 'Старт';
			if(this.isVisible == false) {
				this.toggleVisibility();
			}
		}
	}
}

</script>

<style>
body {
	background: linear-gradient(180deg, rgb(32, 38, 57) 11.4%, rgb(63, 76, 119) 70.2%);
	color: #fff;
	margin: auto;
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
    font-size: 45px;
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

/*.start a{
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
}*/
</style>
