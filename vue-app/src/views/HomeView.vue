<template>
  <div class="home">
    <div class="blockLib border-black border-l h-screen  w-48">
      <block-lib-com v-for="(block, index) in blocks" :key="index" :id="'darg' + block" draggable="true" @dragstart="startDrag($event)"></block-lib-com>
    </div>
    <!-- <div @drop="drop($event)" @dragover.prevent @dragenter.prevent class="normal"></div>
    <div @drop="drop($event)" @dragover.prevent @dragenter.prevent class="small"></div> -->
    <div>
      <div v-for="dropZone in dropZones" :key="dropZone.id" :class="dropZone.type" @drop="drop($event)" @dragleave="dragLeave($event)" @dragover="dragOver($event)" @dragenter=dragEnter($event)></div>
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
      ],
      blocks:[
        1,
        2,
      ],
      count:0
    }
  },
  methods:{
    startDrag(evt){
     const dragElement = document.querySelector('.drag');     
      evt.dataTransfer.setData('text/html', evt.target.id)
      console.log(dragElement, evt.target.id)
      evt.dataTransfer.effectAllowed = "copy";
    },
    dragEnter(evt){
      evt.target.classList.add('showDrop')
    },
    dragLeave(evt){
      evt.target.classList.remove('showDrop')
    },
    dragOver(evt){
      evt.preventDefault();
      evt.dataTransfer.dropEffect = "copy";
    },
    drop(evt){
      console.log(evt.target)
      if(evt.target.childElementCount < 1){
        evt.preventDefault()
        const data = evt.dataTransfer.getData('text/html');
        
        if(data.startsWith('dragged')|| !data){
          return;
        }
        const nodeCopy = document.getElementById(data).cloneNode(true);
        nodeCopy.id = 'dragged' + data + this.count++;
        nodeCopy.classList.remove('libBlock')
        nodeCopy.classList.add('editBlock')
        nodeCopy.querySelector('.blockNav').classList.remove('hideNav')
        nodeCopy.querySelector('.blockNav').classList.add('showNav')
        nodeCopy.querySelector('.showNav .deleteBtn').addEventListener('click', ()=>{
          nodeCopy.remove()
          console.log(nodeCopy)
        })
     
        evt.target.appendChild(nodeCopy)
        evt.target.classList.remove('showDrop')
    }
    },
    showDrop(evt){
      evt.target.classList.add('.showDrop')
    },

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
  overflow: hidden;
}
.normal{
  width: 100vw;
  height: 400px;
  position: relative;
}
.small{
  width: 100vw;
  height: 200px;
  position: relative;

}
.showDrop{
  border: dashed rgba($color: #000000, $alpha: 0.8) 1px;
}
.editBlock{
width: 100%;
}
</style>