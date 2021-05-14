<template>
    <div class="question-box-container">
        <b-jumbotron >
            <template>
                {{currentQuestion.question}}
            </template>

            <hr class="my-4">
            <b-list-group >
                <b-list-group-item 
                v-for="(answer,index) in answers" 
                :key="index" 
                @click='selectAnswer(index)'
                :class="[selectedIndex=== index ? 'selected' : '']"
                >
                    {{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button variant="primary" href="#">Submit</b-button>  
            <b-button @click="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
export default({
    props: {
        currentQuestion:Object,
        next : Function
    },
    data(){
        return {
            selectedIndex: null
        }
    },
    computed: {
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    watech:{
        currentQuestion(){
            this.selectedIndex = null
            this.shuffAnswers()
        }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index
        }
    },
    mounted(){
        console.log(this.currentQuestion)
    }
})
</script>
<style scoped>
.list-group{
    margin-bottom:15px;
}
.list-group-item:hover{
    background-color :rgb(221, 221, 221);
    cursor:pointer;
}
.btn{
    margin:0 5px;
}
.selected{
    background-color:cadetblue;
}
.correct{
    background-color:darkgreen;
}
.incorrent{
    background-color:red;
}
</style>