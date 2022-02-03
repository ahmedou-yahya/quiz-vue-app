<template>
    <div class="result">
        <div class="title">{{results[resultIndex].title}}</div>
        <div class="desc">
            {{results[resultIndex].desc}}
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            
        }
    },
    props:["results","correctAnswers"],
    computed:{
        resultIndex(){
            let index = 0;

            this.results.forEach((element, i) => {
                if(element.min<=this.correctAnswers && element.max>=this.correctAnswers)
                    index=i;
            });
            return index;
        }
    }
}
</script>