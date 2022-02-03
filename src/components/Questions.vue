<template>
    <div class="questions-ctr">
        <div class="progress">
            <div class="bar" :style="{width: `${(questionsAnswered/questions.length)*100}%`}"></div>
            <div class="status">{{questionsAnswered}} out of {{questions.length}} questions answered</div>
        </div>
        <transition-group name="fade">
            <div class="single-question" v-for="(question, index) in questions" :key="index"
            v-show="questionsAnswered===index"
            >
            <div class="question">{{question.q}}</div>
            <div class="answers">
                <div class="answer" v-for="(ans, index) in question.answers" :key="index"
                    @click.prevent="selectAnswer(ans.is_correct)"
                >
                    {{ans.text}}
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
    emits: ["question-answered"],
    methods:{
        selectAnswer(is_correct){
            this.$emit('question-answered', is_correct)
        }
    }
}
</script>