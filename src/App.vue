<template>
  <div>
  <Placar :winCount="this.winCount" :loseCount="this.loseCount"/><br><hr>
    <template v-if="question">
      <h1 v-html="question"></h1>
       <!-- Serviria tambem 'answer in answers' :key="answer" -->
      <template v-for="(answer, index) in answers" :key="index">

        <input type="radio" :value="answer" name="options" 
        v-model="this.chosenAnswer"
        :disabled="this.answerSubmited"/>
        <!-- {{answer}} -->
        <label v-html="answer"></label><br>
      </template>
      <button v-if="!this.answerSubmited"  @click="this.submitAnswer()" class="send" type="button">Send</button>

      <section v-if="this.answerSubmited" class="result">
        <h4 v-if="this.chosenAnswer == this.correctAnswer"
        v-html="'&#9989; Congratulations, the answer is' +  correctAnswer  + '.'">
        
        </h4>
        <h4 v-else v-html="'&#10060; I´m sorry, you picked the wrong answer, the correct is '+ correctAnswer + '.'">
          </h4>
          <button @click="getNewQuestion()" class="send" type="button">Next question</button>

      </section>


    </template>
  </div>
</template>


<script lang="ts">

import axios from 'axios';
import { defineComponent } from 'vue';
import Placar from '@/components/Placar.vue';


export default defineComponent({
  name: 'App',
  components:{
    Placar
  },
  data() {
      return {
         chosenAnswer: undefined,
         question: "",
         incorrectAnswers: [] as string[],
         correctAnswer: "",
         answerSubmited: false,
         winCount:0,
         loseCount:0
      }
  },
  computed:{
    answers(){      //Faz uma copia do essa abordagem retira os valores do array original e os manda para outro array (scrap operator '...')
      let answers = [...this.incorrectAnswers]
//Como o random so utiliza numeros entre 0 e 1, multiplicamos pelo tamanho do array para obter um valor dentro do intervalo
      answers.splice(Math.round(Math.random() * answers.length),0,this.correctAnswer)
      return answers
    }
  },
  methods:{
      submitAnswer(){
        if(!this.chosenAnswer){
          alert("Pick one of the options")
        }else{
          this.answerSubmited=true
          if(this.chosenAnswer == this.correctAnswer){
             this.winCount++
          }else{
            this.loseCount++
          }

        }
      },
      getNewQuestion(){
        this.answerSubmited = false
        this.chosenAnswer=undefined
        this.question=''
        axios
      .get('https://opentdb.com/api.php?amount=10')
      .then(response => { this.question = response.data.results[0].question
      this.incorrectAnswers= response.data.results[0].incorrect_answers
      this.correctAnswer= response.data.results[0].correct_answer})
      }
  },
  created() {
      this.getNewQuestion()
  },

  });
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
