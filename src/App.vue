<script>
import Question from '@/components/Question.vue'

const initialData = [
  {question: 'Q1: Deliver through others', ans: ''},
  {question: 'Q2: Understand others perspective', ans: ''},
  {question: 'Q3: Solve complex problems', ans: ''},
]

export default{
  data(){
    return{
      question: initialData,
      getName: '',
      endpoint: 'https://vue-survey-hcoj.vercel.app',
      choices: ['A. Not Effective', 'B. Neither Ineffective or effective','C. Effective'],
      showSubmit: false
    }
  },
  methods:{
    currentAnswer(value){
      let {quest, ans}=value
      let getQuestion = this.question.findIndex(obj=>obj.question == quest)
      this.question[getQuestion].ans = ans
      console.log(this.question);
      this.getFromDatabase()
      this.renderSubmit()
      
    },
    renderSubmit(){
      let getHasAnswer = []
      this.question.map((item)=>{
        getHasAnswer.push(item.ans != '')
      })
      this.showSubmit = !getHasAnswer.includes(false)
    },
     handleSubmit(){
      this.question.map(async(item)=>{
        if (this.getName === '') {
          alert("We need your name.")
        }else{
          let addData = await fetch(this.endpoint+`/add/${this.getName}/${item.question}/${item.ans}/`)
           let result = await addData.json()
            console.log("Result Submitted:", result);
          this.question = initialData
          this.getName=''
        }
        
      })
     
    },
    async getFromDatabase(){
      
    let getdata = await fetch(this.endpoint+'/post')
    let result = await getdata.json()
    console.log(result);
  }
  },
  components:{
    Question
  },
  
}
</script>


<template>
  <div class="card">
    <div class="name-field">
      <label for="person">Name</label>
      <input id="person" name="person" type="text" v-model="getName" placeholder="Enter your name"/>
    </div>
    <Question 
    v-for="quest in question" 
    :question="quest.question" 
    :choices="choices" 
    @getAnswer="currentAnswer($event)"
    />
    <button class="submit-button" v-if="showSubmit" @click="handleSubmit">Submit</button>

    <!-- Answer table -->
    <p v-for="(item, index) in question" :key="index">{{ item.question }} | {{ item.ans }}</p>
  </div>
</template>