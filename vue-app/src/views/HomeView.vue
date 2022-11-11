<template>
  <div class="home">
    <div class="blockLib border-black border-l h-screen  w-48">
      <block-lib-com @dragstart="startDrag($event)"></block-lib-com>
    </div>
    <!-- <div @drop="drop($event)" @dragover.prevent @dragenter.prevent class="normal"></div>
    <div @drop="drop($event)" @dragover.prevent @dragenter.prevent class="small"></div> -->
    <div>
      <div class=" z-10" v-for="dropZone in dropZones" :key="dropZone.id" :class="dropZone.type" @drop="drop($event)" @dragover.prevent @dragenter="dragEnter($event)" @dragleave="dragLeave($event)"></div>
    </div>
    </div>
</template>

<script>
// @ is an alias to /src
import BlockLibCom from '@/components/BlockLibCom.vue';
export default {
  components: { BlockLibCom },
  name: 'HomeView',
  data(){
    return{
      dropZones:[
        {type:'small', id:1},
        {type:'normal', id:2}
      ]
    }
  },
  methods:{
    startDrag(evt){
     const dragElement = document.querySelector('.drag');     
      evt.dataTransfer.setData('text/html', evt.target.id)
      console.log(dragElement, evt.target.id)
    },
    dragEnter(evt){
      evt.target.classList.add('showDrop')
    },
    dragLeave(evt){
      evt.target.classList.remove('showDrop')
    },
    drop(evt){
      evt.preventDefault()
      const data = evt.dataTransfer.getData('text/html');
      const dragElement = document.querySelector('.drag');
      dragElement.classList.remove('libBlock')
      dragElement.classList.add('editBlock')
      console.log(data, evt.target)
      evt.target.appendChild(document.getElementById(data))
    },
    showDrop(evt){
      evt.target.classList.add('.showDrop')
    }
  }
}
</script>

<style lang="scss" scoped>
.home{
display: flex;
flex-direction: row-reverse;
}
.drops{
  position: relative;
  z-index: -1;
  overflow: hidden;
}
.normal{
  width: 100vw;
  height: 400px;
  position: relative;
  z-index: 99;
  background: rgba($color: #000000, $alpha: 0.1);
}
.small{
  width: 100vw;
  height: 200px;
  position: relative;
  z-index: 99;
  background: rgba($color: #000000, $alpha: 0.1);
}
.showDrop{
  border: dashed rgba($color: #000000, $alpha: 0.8) 1px;
}
.editBlock{
width: 100%;
}
</style>