<template>
    <div  class="question-box-container">
  <b-jumbotron header="BootstrapVue" lead="Bootstrap v4 Components for Vue.js 2">
   

    <template #lead>
     {{ curruntQuestion.question }}
    </template>

    <hr class="my-4">

    <b-list-group>
  <b-list-group-item v-for="(answer , index) in answers" :key="index"
  @click.prevent="selectAnswer(index)" :class="[ 
    !answered && selecedIndex === index ? 'selected' :
    answered  && correctIndex === index ? 'correct' : 
    answered && selecedIndex === index  && correctIndex !==index ?  'incorrect':''
  ] "> 
      {{ answer }}

  </b-list-group-item>
  
</b-list-group>

    <b-button variant="primary" @click="submitAnser" :disabled="selecedIndex===null || answered ">Submit</b-button>
    <b-button @click="next" variant="success" href="#">Next</b-button>
  </b-jumbotron>
</div>
</template>

<script>

import _ from 'lodash'
export default {
props :{
    curruntQuestion:Object,
    next:Function,
    increment: Function
},

data(){

    return {
        selecedIndex:null,
        correctIndex:null,
        shuffledAnswers: [],
        answered:false
    }
},
computed:{

answers() {
    let answers = [...this.curruntQuestion.incorrect_answers]

    answers.push(this.curruntQuestion.correct_answer)

    return answers;
    }
},

watch :{
    curruntQuestion : {
        immediate: true , 
        handler() {
            this.selecedIndex =null
            this.shuffleAnswers(),
            this.answered = false
        }
    }
}, 
methods:{
    selectAnswer(index)
    {
        this.selecedIndex = index;
    },

    shuffleAnswers()
    {
        let answers = [...this.curruntQuestion.incorrect_answers , this.curruntQuestion.correct_answer ] 
        this.shuffledAnswers  = _.shuffle(answers)
        this.correctIndex = this.shuffledAnswers.indexOf(this.curruntQuestion.correct_answer)
    }
    ,
    submitAnser(){
        let isCorerct = false;
        if(this.selecedIndex === this.correctIndex)
        {   
                isCorerct = true;
        }
        this.answered = true

        this.increment(isCorerct) 
    }
    
    
}


}
</script>

<style scoped> 

.list-group

{
    margin-bottom: 15px;
}
.list-group-item:hover{
    background: #EEE;
    cursor: pointer;
}
.btn
{
    margin: 0 5px;
}
.selected {
    background-color: blue;
}

.correct {
    background-color: green;
}

.incorrect {
    background-color: red;
}
</style>
