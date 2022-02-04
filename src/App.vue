<template>
  <Navbar class="navbar" 
  @sart-quiz="startQuiz"
  />
  <div v-if="hasRendred" class="ctr">
    <transition name="fade" mode="out-in">
      <Questions v-if="questionsAnswered<questions.length" 
      :questions="questions"
      :questionsAnswered="questionsAnswered"
      @question-answered="questionAnswered"
      />
      <Result v-else
      :results="results"
      :correctAnswers="correctAnswers"
      />
    </transition>
    
    <button type="button" @click.prevent="reset" class="reset-btn" v-if="questionsAnswered===questions.length">
      Reset
    </button>
</div>
</template>

<script>
import Questions from './components/Questions.vue'
import Result from './components/Result.vue'
import Navbar from './components/Navbar.vue'
const baseUrl = "https://quizapi.io/api/v1/questions"
const apiKey = "bWpqRDg00RMqqmtveK8HmaZYfjd1XTR77fkSe4ow"
export default {
  name: 'App',
  components: {
    Questions,
    Result,
    Navbar
},
  methods:{
    questionAnswered(is_correct){
      if(is_correct)
        this.correctAnswers++;
      
      this.questionsAnswered++;
    },
    reset(){
      this.questionsAnswered = 0;
      this.correctAnswers = 0;
    },
    startQuiz(cat, diff, num){
      this.selectedCategory = cat
      this.selectedDifficulty = diff
      this.selectedNumber = num
      this.getData()
    },
    async getData(){
      try{
        let res = await fetch(`${baseUrl}?apiKey=${apiKey}&category=${this.selectedCategory}&difficulty=${this.selectedDifficulty}&limit=${this.selectedNumber}`)
        this.questions = await res.json();
        console.log(this.questions)
      } catch(error){
        console.log(error)
      }
    }
  },
  computed:{
    hasRendred(){
      return this.questions.length!=0;
    }
  },
  data() {
    return {
      questionsAnswered: 0,
      correctAnswers: 0,
      selectedCategory: '', 
      selectedDifficulty: '', 
      selectedNumber: '',
      questions: [],
      results: [
          {
              min: 0,
              max: 2,
              title: "Try again!",
              desc: "Do a little more studying and you may succeed!"
          },
          {
              min: 3,
              max: 3,
              title: "Wow, you're a genius!",
              desc: "Studying has definitely paid off for you!"
          }
      ]
    }
  },
}
</script>

<style>
* {
  box-sizing: border-box;
}
.navbar{
  padding-bottom: 15px;
  margin-bottom: 10px;
  margin-right: 15px;
}
body {
  font-size: 20px;
  font-family: sans-serif;
  padding-top: 20px;
  background: #e6ecf1;
}

.ctr {
  margin: 0 auto;
  max-width: 600px;
  width: 100%;
  box-sizing: border-box;
  position: relative;
}
.questions-ctr {
  position: relative;
  width: 100%;
}
.question {
  width: 100%;
  padding: 20px;
  font-size: 32px;
  font-weight: bold;
  text-align: center;
  background-color: #00ca8c;
  color: #fff;
  box-sizing: border-box;
}
.single-question {
  position: relative;
  width: 100%;
}
.answer {
  border: 1px solid #8e959f;
  padding: 20px;
  font-size: 18px;
  width: 100%;
  background-color: #fff;
  transition: 0.2s linear all;
}
.answer span {
  display: inline-block;
  margin-left: 5px;
  font-size: 0.75em;
  font-style: italic;
}
.progress {
  height: 50px;
  margin-top: 10px;
  background-color: #ddd;
  position: relative;
}
.bar {
  height: 50px;
  background-color: #ff6372;
  transition: all 0.3s linear;
}
.stats {
  position: absolute;
  top: 15px;
  left: 0;
  text-align: center;
  color: #fff;
  width: 100%;
}
.answer:not(.is-answered) {
  cursor: pointer;
}
.answer:not(.is-answered):hover {
  background-color: #8ce200;
  border-color: #8ce200;
  color: #fff;
}

.title {
  width: 100%;
  padding: 20px;
  font-size: 32px;
  font-weight: bold;
  text-align: center;
  background-color: #12cbc4;
  color: #fff;
  box-sizing: border-box;
}
.desc {
  border: 1px solid #8e959f;
  padding: 20px;
  font-size: 18px;
  width: 100%;
  background-color: #fff;
  transition: 0.4s linear all;
  text-align: center;
}
.fade-enter-from {
  opacity: 0;
}
.fade-enter-active {
  transition: all 0.3s linear;
}
.fade-leave-active {
  transition: all 0.3s linear;
  opacity: 0;
  position: absolute;
}

.reset-btn {
  background-color: #ff6372;
    border: 0;
    font-size: 22px;
    color: #fff;
    padding: 10px 25px;
    margin: 10px auto;
    display: block;
}

.result{ 
  width: 100%;
}


.reset-btn:active, .reset-btn:focus, .reset-btn:hover{
  border: 0;
  outline: 0;
  cursor: pointer;
  
}
</style>
