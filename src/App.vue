<template>
	<div id="app">
		<p id="vidas">Vidas = {{ vidas }}</p>
		
		<h1>Super Quiz</h1>
		<QuizQuestion v-if="questionMode"
		:question="questions[currentQuestion]"
		@answered="showResult"/>
		<QuizResult v-else-if="!showGameOver && !questionMode" class="animate__animated animate__heartBeat" :result="QuizResult" 
			@confirmed="nextQuestion"/>

		
		<FimdeJogo class="animate__animated animate__hinge" v-if="showGameOver"/>
		<button class="btn animate__animated animate__backInRight" v-if="showGameOver" @click="reiniciar">Reiniciar partida</button>
	
		
	</div>
</template>

<script>
import FimdeJogo from './components/FimdeJogo.vue';
import questions from './util/questions';
import QuizQuestion from './components/QuizQuestion.vue';
import QuizResult from './components/QuizResult.vue'; 

export default {
	components:{QuizQuestion,QuizResult,FimdeJogo},
	data(){
		return{
			QuizResult:false,
			questionMode:true,
			questions,
			currentQuestion:0,
			vidas:5,
			showGameOver:false
		}
	},
	methods:{
		showResult(result){
			this.QuizResult = result
			this.questionMode = false
			this.PerdeVida()
		},
		nextQuestion(){
			
			let r = Math.random()* this.questions.length
			this.currentQuestion = parseInt(r)
			this.questionMode = true
			
			
		},
		PerdeVida(){
			
			if(this.QuizResult == false){
				this.vidas -=1
			}
			if(this.vidas ==0){
				this.showGameOver = true
			}
			
		},
		reiniciar(){
			location.reload()
		}

	}

}
</script>

<style>
body {
	background: linear-gradient(to right, rgb(0, 0, 70), rgb(28, 181, 224));
	font-family: 'Oswald', sans-serif;
	color: #FFF;
	height: 100vh;
}
.btn{
	padding: 10px 20px;
	margin: 15px;
	cursor: pointer;
	background-color: rgb(22, 223, 22);
	color: #000000;
	font-size: 1.5rem;
	border: 1px solid transparent;
	border-radius: 5px;
}

.btn:hover{
	font-size: 1.7rem;
	transition: 0.5s;
	
}
#vidas{
	font-size: 1.5rem;
	position: absolute;
	left: 0;
	margin: 50px 200px;
	
}


#app {
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;

	display: flex;
	flex-direction: column;
	align-items: center;
}

#app h1 {
	font-weight: 200;
	font-size: 4rem;
}

@keyframes flip-out {
	from { transform: rotateY(0deg); }
	to { transform: rotateY(90deg); }
}

@keyframes flip-in {
	from { transform: rotateY(90deg); }
	to { transform: rotateY(0deg); }
}

.flip-enter-active {
	animation: flip-in 0.3s ease;
}

.flip-leave-active {
	animation: flip-out 0.3s ease;
}
</style>
