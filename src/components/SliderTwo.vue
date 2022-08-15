<template>
  <div class="slider2" :style="{'color' : progressColor}">
    <span class="indicator">{{min}}</span>
    <div class="range-slider" ref="slider" :style="sliderStyles">
        <input type="range" :min="min" :max="max" :value="modelValue" @input="$emit('update:modelValue', Number($event.target.value))" />
    </div>
    <span class="indicator">{{max}}</span>
  </div>
</template>

<script>
import { ref, computed} from 'vue';
export default{
    props:{
        max:{
            type:Number
        },
        min:{
            type:Number
        },
        modelValue:{
            type:Number
        },
        sliderBackground:{
            type:String,
            default:"#C6AEE7",
        },
        progressColor:{
            type:String,
            default:"#6200EE"
        }
    },

    setup(props, ctx){
        const slider = ref(null)
        const percent = ref('')
        
        const sliderWidth = computed(() => {
            percent.value = ((props.modelValue - props.min)/(props.max - props.min)) * 100;
            return percent.value+'%';
        });

        const sliderStyles = computed(()=>{
            return {
                '--progressColor' : props.progressColor,
                '--sliderBackground' : props.sliderBackground,
                '--thumbShadow' : props.sliderBackground + 'ae',
                '--slider': sliderWidth.value
            }
        })

        return{
            sliderWidth,
            slider,
            sliderStyles
        }
    }
}
   
</script>

<style scoped>
.slider2{
    border-radius: 5px;
    background: #fff;
    padding:1.5rem 1rem;
    display: flex;
    gap:20px ;
    align-items: center;
    width:80%;
    max-width:400px;
    margin-bottom: 2rem;
}
.range-slider{
  --slider: 50%;
  flex-grow: 1;
  position: relative;
  display: flex;
  align-items: center;
}
input[type=range] {
  -webkit-appearance: none;
  margin: 18px 0;
  width:100%;
  height: 8px;
  background: var(--sliderBackground);
  background-image: linear-gradient(to right, var(--progressColor) 0%, var(--progressColor) 100%);
  border-radius: 5px;
  background-size: var(--slider) 100%;
  background-repeat: no-repeat;
}
input[type=range]:focus {
  outline: none;
}

input[type=range]::-webkit-slider-runnable-track{
  width: 100%;
  height: 8px;
  cursor: pointer;
}

input[type=range]::-webkit-slider-thumb{
  border: none;
  height: 30px;
  width: 30px;
  border-radius: 50%;
  background: var(--progressColor);
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -11px;
  position: relative;
  z-index: 200;
}

input[type=range]:focus::-webkit-slider-thumb {
    box-shadow: 0 0 0 8px var(--thumbShadow);
}

input[type=range]:focus::-moz-range-thumb {
    box-shadow: 0 0 0 8px var(--thumbShadow);
}

input[type=range]::-moz-range-track {
  width: 100%;
  height: 8px;
  cursor: pointer;
}
input[type=range]::-moz-range-thumb {
  border: none;
  height: 30px;
  width: 30px;
  border-radius: 50%;
  background:var(--progressColor);
  cursor: pointer;
  position: relative;
  z-index: 300;
}
.indicator{
  font-weight: 500;
  font-size:1rem;
}

</style>