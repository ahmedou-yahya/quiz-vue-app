<template>
    <nav class="container">
      <div class="icon">Quiz App</div>  
      <div class="filters">
          <div class="category">
              <label for="">category</label>
              <select name="category" id="" v-model="selectedCategory">
                  <option disabled value="">select category</option>
                  <option>Code</option>
                  <option>Docker</option>
                  <option>SQL</option>
                  <option>Linux</option>
                  <option>DevOps</option>
              </select>
          </div>
          <div class="difficulty">
                <label for="">Difficulty</label>
                <select name="difficulty" id="" v-model="selectedDifficulty">
                  <option disabled value="">select difficuly</option>
                  <option>Easy</option>
                  <option>Medium</option>
                  <option>Hard</option>
                </select>
          </div>
          <div class="numbers">
              <label for="">Number of questions</label>
                <select name="number" id="" v-model="selectedNumber">
                  <option disabled value="">select number</option>
                  <option>5</option>
                  <option>10</option>
                  <option>15</option>
                  <option>20</option>
                </select>
          </div>
          <div class="button">
            <button :disabled="!validInputs" id="button" type="submit" @click.prevent="startQuiz(selectedCategory, selectedDifficulty, selectedNumber)">Start</button>
          </div>
      </div>
    </nav>
</template>

<script>
export default {
    data() {
        return {
            selectedCategory: '',
            selectedDifficulty: '',
            selectedNumber: '',
        }
    },
    emits:["sart-quiz"],
    methods:{
        startQuiz(selectedCategory, selectedDifficulty, selectedNumber){
            this.$emit("sart-quiz", selectedCategory, selectedDifficulty, selectedNumber)
    }
    },
    computed:{
       validInputs(){
           return (this.selectedCategory.length!=0 && this.selectedDifficulty.length!=0 && this.selectedNumber.length!=0)
       } 
    }
}
</script>
<style scoped>
.container{
    display: flex;
    justify-content: space-between;
}
.filters{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.button{
    margin: 10px;
}
#button{
    background-color: #00CA8C;
    padding: 12px 24px;
    border-radius: 15%;
    font-size:medium;
    color: white;
}
#button:hover{
    cursor: pointer;
}
</style>