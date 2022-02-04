<template>
    <div class="questions-ctr">
        <div class="progress">
            <div class="bar" :style="{width: `${(questionsAnswered/questions.length)*100}%`}"></div>
            <div class="stats">{{questionsAnswered}} out of {{questions.length}} questions answered</div>
        </div>
        <transition-group name="fade">
            <div class="single-question" v-for="(question, index) in validQuestions" :key="index"
            v-show="questionsAnswered===index"
            >
                <div class="question">{{question.question}}</div>
                <div class="answers">
                    <div class="answer" v-for="(ans, i) in answers" :key="i"
                        @click.prevent="selectAnswer(question.correct_answers[index])"
                    >
                        {{ans[i]}}
                    </div>
                </div>
            </div>
        </transition-group>
    </div>
</template>

<script>
export default {
    props: ["questions", "questionsAnswered"],
    data() {
        return {
            
        }
    },
    computed:{
        validQuestions(){
            return this.questions.filter(e=> e.answers!=null)
        },
        answers(){
            let arr = this.questions.forEach(element => element.answers);
            console.log(arr)
            return arr;
        }
    },
    emits: ["question-answered"],
    methods:{
        selectAnswer(is_correct){
            this.$emit('question-answered', is_correct)
        }
    }
}
</script>