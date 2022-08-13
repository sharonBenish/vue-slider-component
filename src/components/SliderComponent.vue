<template>
  <div id="slider-component">
    <div class="indicator">{{min}}</div>
    <div class="range-slider" ref="slider">
      <div class="progress"></div>
      <input type="range" :min="min" :max="max" :value="value" @input="emitChange" />
    </div>
    <div class="indicator">{{max}}</div>
  </div>
</template>

<script>
import { onMounted, ref, onUpdated} from 'vue';

export default{
    emits:['value-changed'],
    props:{
        max:{
            type:Number
        },
        min:{
            type:Number
        },
        value:{
            type:Number
        }
    },

    setup(props, ctx){
        const slider = ref(null)
        const percent = ref('')

        const emitChange = (e)=>{
            const value = Number(e.target.value);
            ctx.emit('value-changed', value)
        }
        
        onMounted(()=>{
            updateSliderProgress()
        })
        onUpdated(()=>{
            updateSliderProgress()
        })

        function updateSliderProgress(){
            percent.value = ((props.value - props.min)/(props.max - props.min)) * 100;
            slider.value.style.setProperty('--slider', `${percent.value}%`);
        }

        return{
            slider,
            emitChange
        }
    }
}
   
</script>

<style scoped>
#slider-component{
    display: flex;
    gap:20px ;
    align-items: center;
    width:80%;
    max-width:400px;
    padding: 1.5rem 1rem;
    border:2px solid #fff;
    border-radius: 5px;
    margin-bottom: 2rem;
}
.range-slider{
  flex-grow: 1;
  position: relative;
  --slider: 50%;
  display: flex;
  align-items: center;
}

input[type=range] {
  -webkit-appearance: none;
  margin: 18px 0;
  width:100%;
  height: 0.5px;
  background: #fff;
  /*background-image: linear-gradient(to right, blue 0%, blue var(--sliderPercent), #eaeefb var(--sliderPercent),#eaeefb 100%);*/
  background-image: linear-gradient(to right, #fff 0%, #fff 100%);
  background-size: var(--slider) 100%;
  background-repeat: no-repeat;
}

.progress{
  width:var(--slider);
  height:3px;
  background: #fff;
  content: "";
  position: absolute;
  top:50%;
  left:0;
  transform: translateY(-50%);
  border-radius: 1.3px;
}

input[type=range]:focus {
  outline: none;
}

input[type=range]::-webkit-slider-runnable-track{
  width: 100%;
  height: 0.5px;
  cursor: pointer;
  border-radius: 1.3px;
  /*border: 0.2px solid #fff;*/
}
input[type=range]::-webkit-slider-thumb{
  border: 2px solid #fff;
  height: 30px;
  width: 30px;
  border-radius: 50%;
  background: var(--background-color);
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -14.75px;
  position: relative;
  z-index: 200;
}

input[type=range]:focus::-webkit-slider-runnable-track {
}
input[type=range]::-moz-range-track {
  width: 100%;
  height: 0.5px;
  cursor: pointer;
  border-radius: 1.3px;
}
input[type=range]::-moz-range-thumb {
  border: 2px solid #fff;
  height: 30px;
  width: 30px;
  border-radius: 50%;
  background:var(--background-color);
  cursor: pointer;
  position: relative;
  z-index: 300;
}

.indicator{
  font-weight: 500;
  font-size:0.8rem;
  color:#fff
}
</style>