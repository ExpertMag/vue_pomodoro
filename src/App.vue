<template>
	<section id="wrapper">
		<div class="" v-if="!isVisibleDescription">
			<div class="indentation flex">
				<p style="padding-right: 5px;">Текущее состояние: </p>
				<button @click="switchState">{{ isWork ? 'Работа' : 'Отдых' }}</button>
			</div>
			<div class="indentation flex">
				<button @click="subtractMinutes">Минус</button>
				<button @click="addMinutes">Плюс</button>
			</div>
			<div class="timer indentation flex"><p class="text">{{ hours }}:{{ minutes }}:{{ seconds }} </p></div>
			<div class="start indentation">
				<button @click="startTimer" v-show="isVisible">{{ buttonText }}</button>
				<button @click="pauseTimer" v-show="!isVisible">Пауза</button>
				<button @click="resetTimer" v-show="!isVisibleReset">Сбросить таймер</button>
				<!--<span class="start-timer" @click="startTimer" :disabled="disabled == 1 ? true : false">Начать</span>-->
			</div>
		</div>
		<FormDescription v-if="isVisibleDescription" @formSubmitted="handleFormSubmitted"/>
	</section>
</template>

<script setup>
import FormDescription from '@/components/FormDescription.vue';
import { ref, onMounted } from 'vue'

const dateNow = ref(new Date())
const hours = ref(null)
const minutes = ref(null)
const seconds = ref(null)
const base = ref([])
const isWork = ref(true)
const isVisible = ref(true)
const isVisibleReset = ref(true)
const isVisibleDescription = ref(false)
const counterId = ref(null)
const buttonText = ref('Старт')
const updateTime = () => {
	hours.value = dateNow.value.getHours() < 10 ? "0" + dateNow.value.getHours() : dateNow.value.getHours();
	minutes.value = dateNow.value.getMinutes() < 10 ? "0" + dateNow.value.getMinutes() : dateNow.value.getMinutes();
	seconds.value = dateNow.value.getSeconds() < 10 ? "0" + dateNow.value.getSeconds() : dateNow.value.getSeconds();
}
const toggleVisibility = () => {
	isVisibleReset.value = !isVisibleReset.value;
}

onMounted(() => {
	dateNow.value.setHours(0, 0, 5);
	updateTime();
	// buttonText.value = document.querySelector('.start.indentation button')

})


function addMinutes() {
	if(isWork.value == true) {
		if(dateNow.value.getHours() <= 1) {
		dateNow.value.setMinutes(dateNow.value.getMinutes() + 5)
		updateTime();
		}
	} else {
		if(dateNow.value.getHours() <= 1) {
		dateNow.value.setMinutes(dateNow.value.getMinutes() + 1)
		updateTime();
		}
	}
}

function subtractMinutes() {
	if(isWork.value == true) {
		if(dateNow.value.getMinutes() > 10 && dateNow.value.getHours() == 0 || dateNow.value.getHours() == 1 || dateNow.value.getHours() == 2) {
		dateNow.value.setMinutes(dateNow.value.getMinutes() - 5)
		updateTime();
		}
	} else {
		if(dateNow.value.getMinutes() > 5 && dateNow.value.getHours() == 0 || dateNow.value.getHours() == 1 || dateNow.value.getHours() == 2) {
		dateNow.value.setMinutes(dateNow.value.getMinutes() - 1)
		updateTime();
		}
	}
}
function startTimer() {
	toggleVisibility();
	isVisible.value = false;

	let time = {
		id: base.value.length,
		hour: hours.value,
		minute: minutes.value,
		second: seconds.value
	};
	base.value.push(time);

	
	const minusTimer = () => {
		dateNow.value.setSeconds(dateNow.value.getSeconds() - 1);
		updateTime();
		if(hours.value == 0 && minutes.value == 0 && seconds.value == 0) {
			clearInterval(counterId.value);
			isWork.value = !isWork.value;
			if(isWork.value == false) {
				isVisibleDescription.value = !isVisibleDescription.value;
			}
			resetTimer();
		}
	}
	counterId.value = setInterval(minusTimer, 1000);
	

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
}

/*function toggleVisibility() {
	isVisible.value = !isVisible.value;
	disabled.value = !disabled.value;
}*/
function pauseTimer() {
	clearInterval(counterId.value);
	isVisible.value = !isVisible.value;
	buttonText.value = 'Продолжить';
}
function resetTimer() {
	clearInterval(counterId.value);
	if(isWork.value == true) {
		dateNow.value.setHours(0, 0, 10);
	} else {
		dateNow.value.setHours(0, 0, 6);
	}
	updateTime();
	isVisible.value = true;
	buttonText.value = 'Старт';
	if(isVisibleReset.value == false) {
		toggleVisibility();
	}
}
function handleFormSubmitted() {
	isVisibleDescription.value = !isVisibleDescription.value;
}
function switchState(){
	isWork.value = !isWork.value;
}


/*export default {
/*	components: {
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
			isVisibleDescription: false,
			counterId: null,
			buttonText: null,
		}
	},
	mounted() {
		dateNow.value.setHours(0, 0, 5);
		updateTime();
		buttonText.value = document.querySelector('.start.indentation button')
	},*/
	/*methods: { */
		/*updateTime() {
		hours.value = dateNow.value.getHours() < 10 ? "0" + dateNow.value.getHours() : dateNow.value.getHours();
		minutes.value = dateNow.value.getMinutes() < 10 ? "0" + dateNow.value.getMinutes() : dateNow.value.getMinutes();
		seconds.value = dateNow.value.getSeconds() < 10 ? "0" + dateNow.value.getSeconds() : dateNow.value.getSeconds();
		},*/
		/*function addMinutes() {
			if(isWork.value == true) {
				if(dateNow.value.getHours() <= 1) {
				dateNow.value.setMinutes(dateNow.value.getMinutes() + 5)
				updateTime();
				}
			} else {
				if(dateNow.value.getHours() <= 1) {
				dateNow.value.setMinutes(dateNow.value.getMinutes() + 1)
				updateTime();
				}
			}
		},
		subtractMinutes() {
			if(isWork.value == true) {
				if(dateNow.value.getMinutes() > 10 && dateNow.value.getHours() == 0 || dateNow.value.getHours() == 1 || dateNow.value.getHours() == 2) {
				dateNow.value.setMinutes(dateNow.value.getMinutes() - 5)
				updateTime();
				}
			} else {
				if(dateNow.value.getMinutes() > 5 && dateNow.value.getHours() == 0 || dateNow.value.getHours() == 1 || dateNow.value.getHours() == 2) {
				dateNow.value.setMinutes(dateNow.value.getMinutes() - 1)
				updateTime();
				}
			}
		},
		startTimer() {
			this.toggleVisibility();
			isVisible.valueReset = false;
			if(isWork.value == true) {
				textIsWork.value = 'Работа'
			} else {
				textIsWork.value = 'Отдых'
			}
			let time = {
				id: base.value.length,
				hour: hours.value,
				minute: minutes.value,
				second: seconds.value
			};
			base.value.push(time);

			
			const minusTimer = () => {
				dateNow.value.setSeconds(dateNow.value.getSeconds() - 1);
				updateTime();
				if(hours.value == 0 && minutes.value == 0 && seconds.value == 0) {
					clearInterval(this.counterId);
					isWork.value = !isWork.value;
					if(isWork.value == false) {
						isVisibleDescription.value = !isVisibleDescription.value;
					}
					this.resetTimer();
					textIsWork.value = 'Отдых';
					isWork.value == false ? textIsWork.value = 'Отдых' : textIsWork.value = 'Работа';
				}
			}
			this.counterId = setInterval(minusTimer, 1000);
			
			if(textIsWork.value == 'Отдых') {
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
/*		},
		toggleVisibility() {
			isVisible.value = !isVisible.value;
			disabled.value = !disabled.value;
		},
		pauseTimer() {
			clearInterval(this.counterId);
			isVisible.value = !isVisible.value;
			disabled.value = !disabled.value;
			buttonText.value.innerText = 'Продолжить';
		},
		resetTimer() {
			clearInterval(this.counterId);
			if(isWork.value == true) {
				dateNow.value.setHours(0, 0, 10);
			} else {
				dateNow.value.setHours(0, 0, 6);
			}
			updateTime();
			isVisible.valueReset = true;
			buttonText.value.innerText = 'Старт';
			if(isVisible.value == false) {
				this.toggleVisibility();
			}
		},
		handleFormSubmitted() {
		isVisibleDescription.value = !isVisibleDescription.value;
		},
		switchState(){
			isWork.value = !isWork.value;
			console.log(isWork.value)
		}
	}
}*/

</script>

<style>
body {
	background: linear-gradient(180deg, rgb(32, 38, 57) 11.4%, rgb(63, 76, 119) 70.2%);
	color: #fff;
	margin: auto;
}

.flex {
	display: flex;
    justify-content: center;
	align-items: center;
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
