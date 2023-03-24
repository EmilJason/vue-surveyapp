<template>
    <div class="quest">
        <h3>{{ question }}</h3>

        <button 
        @click="()=>submitbutton(question,'A. Not Effective')"
        :class="[isSelectedA ? 'selected' : null]" >
            A. Not Effective
        </button>
        <button 
        @click="()=>submitbutton(question,'B. Neither Ineffective or effective')"
        :class="[isSelectedB ? 'selected' : null]" >
             B. Neither Ineffective or effective
        </button>
        <button 
        @click="()=>submitbutton(question,'C. Effective')"
        :class="[isSelectedC ? 'selected' : null]" >
            C. Effective
        </button>
    </div>
</template>

<script>
export default{
    data(){
        return{
            isSelectedA: false,
            isSelectedB: false,
            isSelectedC: false,
            
        }
    },
    methods:{
        submitbutton(quest,ans){
            // to parent component
            this.$emit('getAnswer', {quest,ans})
            switch (ans) {
                case 'A. Not Effective':
                    this.isSelectedA = true
                    this.isSelectedB= false
                    this.isSelectedC= false
                    break;
                case 'B. Neither Ineffective or effective':
                    this.isSelectedA = false
                    this.isSelectedB= true
                    this.isSelectedC= false
                    break;
                case 'C. Effective':
                    this.isSelectedA = false
                    this.isSelectedB= false
                    this.isSelectedC= true
                    break;
            
                default:
                    this.isSelectedA = false
                    this.isSelectedB= false
                    this.isSelectedC= false
                    break;
            }
        },
        
    },
    props: ['question','choices', ]
}
</script>