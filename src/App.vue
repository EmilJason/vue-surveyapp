<script>
import Question from '@/components/Question.vue'


const initialData = [
  {question: 'Q1. Deliver through others', ans: ''},
  {question: 'Q2. Understand others perspective', ans: ''},
  {question: 'Q3. Solve complex problems', ans: ''},
]

export default{
  data(){
    return{
      question: initialData,
      getName: '',
      endpoint: 'https://vue-survey-xgr8.vercel.app',
      choices: ['A. Not Effective', 'B. Neither Ineffective or effective','C. Effective'],
      showSubmit: false,
      list: [],
      showList: false
    }
  },
  methods:{
    currentAnswer(value){
      let {quest, ans}=value
      let getQuestion = this.question.findIndex(obj=>obj.question == quest)
      this.question[getQuestion].ans = ans
      console.log(this.question);
      this.renderSubmit()
      
    },
    renderSubmit(){
      let getHasAnswer = []
      this.question.map((item)=>{
        getHasAnswer.push(item.ans != '')
      })
      this.showSubmit = !getHasAnswer.includes(false)
    },
      async handleSubmit(){   
        
        console.log("to json", JSON.stringify(this.question));
        if (this.getName === '') {
          alert("We need your name.")
        }else{
           let addData = await fetch(this.endpoint+'/add',{
            method: "POST",
            headers:{
              "Content-type": "application/json"
            },
            body:JSON.stringify({
              name: this.getName,
              data: JSON.stringify({...this.question})
            })
           })
           console.log(addData);  
          }
          this.getName=''
          this.question = initialData
          this.showSubmit = false
          this.getFromDatabase()
    },
    async getFromDatabase(){  
      let getdata = await fetch(this.endpoint+"/post")
      let result = await getdata.json()
      this.list = result
    }
  },
  components:{
    Question
  },
  mounted(){
    this.getFromDatabase()
  }
}
</script>


<template>
  <div class="card">
    <h2>Survey</h2>
    <div class="name-field">
      <label for="person">Name:</label>
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
   
      <table >
        <tr>
          <th>Name</th>
          <th>Answers</th>
        </tr>
        <tr v-for="item in list">
          <td>{{ item.name }}</td>
          <td>{{ item.data }}</td>
        </tr>
      </table>
  
  </div>
</template>