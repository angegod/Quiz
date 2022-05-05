<script setup>
import {ref,computed} from 'vue'

const user=ref('');

const questions=ref([//題庫
  {
  question:'What is Vue JS?',
  answer:0,//答案的索引值
  options:[
    'A front and framework',
    'A library',
    'An ice cream maker'
  ],selected: null

},{
  question:'What is VueX?',
  answer:2,
  options:[
    'Vue with an X',
    'A cheese selection',
    'State management library'
  ],selected: null
},{
  question:'What is Vue Router user for?',
  answer:1,
  options:[
    'Walking in space ',
    'A routing library for Vue JS',
    'Burgar sauce',
    'Quizzes'
  ],selected: null
},{
  question:'Test question',
  answer:1,
  options:[
    'Walking in space ',
    'A routing library for Vue JS',
    'Burgar sauce',
    'Quizzes'
  ],selected: null
}])

const quizCompleted=ref(0)//0代表設置玩家 1代表作答 2代表結算

const currentQuestion=ref(0)

const score= computed(()=>{//得分這個動作
  let value=0
  questions.value.map(q=>{   //map很像for迴圈 可以針對每一個陣列的數組做運算
    if(q.selected==q.answer){
      value++
    }
  })
  return value
})


const getCurrentQuestion=computed(()=>{
  let question =questions.value[currentQuestion.value]//當前的題目
  question.index=currentQuestion.value
  return question
})

const SetAnswer=evt=>{
  questions.value[currentQuestion.value].selected=evt.target.value//設定當前題目哪個選項被選擇
  evt.target.value=null//刷新選擇
}

const NextQuestion=()=>{
  if(currentQuestion.value<questions.value.length-1){
    currentQuestion.value++
  }else{
    quizCompleted.value=2;

  }
}

const setUser=()=>{//直接用vmodel綁定不需要透過方法呼叫設置 除非要多做變化
  
}

const enter=()=>{
  console.log(user.value);
  if(user.value!='')
    quizCompleted.value=1;
  else
    alert("The User name Required!!")
}



</script>

<template>
  <main class="app">
    <h1>The Quiz</h1>
  <section class="user-info" v-if="quizCompleted==0">
    <input type="text" v-model="user" placeholder="username" class="setUser" v-on:keyup.enter="enter"/>
    <button 
      @click="enter"
      class="setUserbtn"
      :disabled="!user"
    >Enter</button>
  </section>

  <section class="quiz" v-if="quizCompleted==1">
    <div class="quiz-info">
      <span class="question">{{getCurrentQuestion.question}}</span>
      <span class="score">Score:{{score}}/{{questions.length}}</span>

      <div class="options">
        <label 
          v-for="(option,index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected ==index
                ?index ==getCurrentQuestion.answer
                  ? 'correct'
                  : 'wrong'
                :
                  ''
          }${
            getCurrentQuestion.selected!=null&&index!=getCurrentQuestion.selected
              ? 'disabled'
              : ''

          }`">
        <input type="radio" :name="getCurrentQuestion.index" :value="index" v-model="getCurrentQuestion.selected"
        :disabled="getCurrentQuestion.selected"
        @change="SetAnswer"
        />
        <span>{{option}}</span>
        </label>
      </div>
      <button
        @click="NextQuestion"
        :disabled="!getCurrentQuestion.selected ">
        {{getCurrentQuestion.index==questions.length-1
            ? 'Finish'
            :getCurrentQuestion.selected==null
              ? 'select an option'
              : 'Next Question'}}

      </button>
    </div>
  </section>
  <section v-else-if="quizCompleted==2">
    <h2>You:<label class="username">{{user}}</label> have finished the quiz!!</h2>
    <p>Your score is {{score}}/{{questions.length}}</p>
  </section>
 
  </main>
  
</template>

<style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Montserrat',sans-serif;
  }

  body{
    background-color: #271C36;
    color:#FFF;
  }

  .app{
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem;
    min-height: 100vh;
  }
  h1{
    font-size: 2rem;
    margin-bottom: 2rem;
  }
  .quiz{
    background-color: #382a4b;
    padding: 1rem;
    width:100%;
    max-width: 640px;
    border-radius: 0.5rem;
  }
  .quiz-info{
    
    justify-content: space-between;
    margin-bottom:1rem;
  }

  .quiz-info .question{
    color:#8F8F8F;
    font-size: 1.25rem;
  }
  .quiz-info .score{
    float: right;
    color:#FFF;
    font-size: 1.25rem;
  }

  .options{
    margin-bottom: 1rem;
  }

  .option{
    display: block;
    padding: 1rem;
    background-color: #271C36;
    margin-bottom: 0.5rem;
    border-radius: 0.5rem;
    cursor: pointer;

  }
  .option:hover{
    background-color:#2d213f;

  }
  .option.correct{
    background-color: #2cce7d;
  }
  .option.wrong{
    background-color: #ff5a5f;
  }
  .option:last-of-type{
    margin-bottom: 0;
  }

  .option.disabled{
    opacity: 0.5;
  }

  .option input{
    display: none;
  }

  button{
    appearance: none;
    outline: none;
    border: none;
    cursor: pointer;

    padding:0.5rem 1rem;
    background-color: #2cce7d;

    color: #2d213f;
    font-weight: 700;
    text-transform: uppercase;
    font-size: 1.25rem;
    border-radius: 0.5rem;
  }

  button:disabled{
    opacity: 0.5;
  }

  h2{
    font-size: 2rem;
    margin-bottom: 2rem;
    text-align: center;
  }

  p{
    color: #8f8f8f;
    font-size: 1.25rem;
    text-align: center;
  }
  button.setUserbtn{
    height: 40px;
  }
  input.setUser{
    height: 40px;
    border-radius: 0.5rem;
    text-align: center;
  }
  label.username{
    color: #ff5a5f;
  }
</style>
