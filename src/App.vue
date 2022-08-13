<template>
  <div id="main">
    <div class="output">Age: {{ age }}</div>
    <SliderComponent 
    :value="age" 
    :min="0" 
    :max="20"
    @value-changed="onValueChange"
     />
     <SliderTwo :min="0" :max="20" v-model="age" />
     <!--when the v-model directive is used on a custom component, it recieves a prop called modelValue automatically and listens to an event called update:modelValue-->
  </div>
</template>

<script>
import { ref } from 'vue';
  import SliderComponent from './components/SliderComponent.vue';
  import SliderTwo from './components/SliderTwo.vue';
  export default{
    components:{
    SliderComponent,
    SliderTwo
},
    setup(){
      const age = ref(2);
      const onValueChange = (value)=>{
        age.value = value
      }
      return{
        age,
        onValueChange
      }
    }
  }
  
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');
*{
  padding:0;
  margin:0;
  box-sizing: border-box;
  font-family: 'Roboto', sans-serif;
}
#app{
  --background-color:#684BFF;
  background:var(--background-color) ;
}

#main{
  width:100%;
  min-height:100vh ;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.output{
  font-size:2rem;
  color:#fff;
  padding:1rem 0;
}
</style>
