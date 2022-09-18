<template>
  <h1> The Quiz </h1> 
  <div id="main">
    <section v-if="!quizCompleted" class='section1'>
      <div class="header">
        <span>{{ getCurrentQuestion }}</span>
        <span class="score"> Score {{ score }} / {{ questions.length }} </span>
      </div>

      <div class="options">
        <label 
          v-for="(option,index) in getCurrentoptions" 
          :key="index" 
          :class="`option 

                        ${ getSelectedOptions == index ? index == getCorrectanswer
                              ? 'Correct' : 'Wrong' : '' }

                        ${ getSelectedOptions != null && index != getSelectedOptions
                              ? 'disabled' : '' } 
          ` "> 
          <input 
            type="radio" 
            :name="index" 
            :value="index"
            v-model='getSelectedOptions'
            :disabled='getSelectedOptions'
            @change="setanswer"> 
          <span>{{option}}</span> 
        </label>

        <div>
          <button @click="nextQuestion" :disabled='!getSelectedOptions'>
            {{ questions.length  == this.empty_list.length
                  ? 'Finish' 
                  :  getSelectedOptions == null
                  ? 'Select an Option'
                  : 'Next Question' 
                  }}
          </button>
        </div>

      </div>
    </section>

    <section v-else class='section2'>
      <h2>Your have Completed the Quiz</h2>
      <p>Your Score is {{ score }} / {{ questions.length }} </p>
    </section>

  </div> 
</template>


<script >

import {allquestions} from './assets/allquestions'

export default {
  
  data() {
    const questionno = Math.floor(Math.random() * allquestions.length )

return {
  questions : allquestions,
  quizCompleted :false,
  currentquestion : questionno ,
  questionlength : allquestions.length,
  totquestion : 0,
  empty_list : [questionno]
  }
  },

  computed : {
    score() {
      let value = 0
      this.questions.map(q =>
      {
      if (q.Selected == q.answer) 
      { value++ }
      })
      return value
    },
    getCurrentQuestion() {
      let question = this.questions[this.currentquestion]
      // question.index = this.currentquestion
      return question.question
    },
    getCurrentoptions() {
      let question = this.questions[this.currentquestion]
      return question.options
    },
    getSelectedOptions() {
      let question = this.questions[this.currentquestion]
      return question.Selected
    },
    getCorrectanswer() {
      let question = this.questions[this.currentquestion]
      return question.answer
    }
  },
  methods : {

    setanswer(evt) 
    {
    this.questions[this.currentquestion].Selected = evt.target.value
    evt.target.value = null
    },

    checkingFunction(no) {
          if (this.empty_list.includes(no)) 
          {
            // console.log('Is already there')
            this.nextQuestion();
          }
          else {
            // console.log('no there')
            console.log(this.empty_list)
            this.empty_list.push(no)
            this.totquestion++
            console.log(this.totquestion,'There are ')
            this.next(no)
          }
    },

    nextQuestion() {

      let questionno = Math.floor(Math.random() * allquestions.length )
      
      if ( this.empty_list.length == 10 ) {
          this.quizCompleted = true
      }
      this.checkingFunction(questionno)
      
    },

    next (no) {
      if ( this.empty_list.length <= 10 ) 
        {
          this.currentquestion = no
        } 
        else 
        {
        this.quizCompleted = true
        }
    }
                  
  }
}


// const questions = ref([
//   { question : 'What is Vue' ,
//     answer : 0,
//     options : [
//       'A front end framwork',
//       'A library',
//       'An iceCream Maker'
//     ],
//     Selected : null
//   },
//   { question : 'Who is Kavin' ,
//     answer : 2,
//     options : [
//       'A programmer',
//       'A Front end Developer',
//       'A Student'
//     ],
//     Selected : null
//   },
//   { question : 'What is python' ,
//     answer : 2,
//     options : [
//       'A library',
//       'A language',
//       'An scripting language'
//     ],
//     Selected : null
//   },
//   { question : 'What is React' ,
//     answer : 2,
//     options : [
//       'A framework  ',
//       'A library',
//       'Both'
//     ],
//     Selected : null
//   }

// ])

// const quizCompleted = ref(false)

// const currentquestion = ref(0)

// const score = computed(() => {
  
//   let value = 0
//   questions.value.map(q => {
//     if (q.Selected == q.answer) {
//       value++
//     }
//   })
//   return value
// })

// const getCurrentQuestion = computed(() => {
//   let question = question.value[currentquestion.value]
//   question.index = currentquestion.value
//   return question
// })

// const setanswer = evt => {
//   questions.value[currentquestion.value].Selected = evt.target.value
//   evt.target.value = null
// }

// const nextQuestion = () => {
//   if ( currentquestion.value > questions.value.length - 1 ) {
//     currentquestion.value++
//   } else {
//     quizCompleted.value = true
//   }
// }

</script>

<style>

body { 
  background-color: #26282B
}

#main {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    border-radius: 15px;
    margin: 2.5em 18rem 0rem 18rem;
    padding: 1rem 3rem 1rem 3rem;
    background-color: #382a4b;
}

h1 {
  display: flex;
  text-align: center;
  justify-content: center;
  margin-top: 60px;
  color: white;
}

.header {
  display: flex;
  justify-content: space-between;
  color: #8f8f8f;
  margin: 20px 0 0 0;
  font-size: 1rem;
}

.section1 {
  display: flex;
  flex-direction: column;
  row-gap: 20px;

}
.option {
  display: block;
  padding: 1rem;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 7px;
  color: white;
}

button {
  margin: 20px 0 10px 0;
  padding: 5px 15px 5px 15px;
  border-radius: 5px;
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  background-color: #2cce7d;
  color: #2d213f;
  font-size: 1rem;
}

button.disabled {
  opacity: 0.5;
}

input {
  margin-right: 10px;
  display: none;
}

.score {
   color: white;
}

.option:hover {
  background-color: #2d213f;
}

.option.Correct {
  background-color: #2cce7d;
}

.option.Wrong {
  background-color: crimson;
}
.option.disabled {
  opacity: 0.5;
}

.section2 h2 , p {
  display: flex;
  justify-content: center;
  text-align: center;
  color:white
}


@media only screen and (max-width: 600px) {
  #main {
    margin: 0;
}
}

</style>
