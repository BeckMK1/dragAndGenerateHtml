<template>
  <div class="home">
    <div class="blockLib border-black border-l h-screen fixed">
      <block-lib-com v-for="(block, index) in blocks" :key="index" :id="'darg' + block" draggable="true" @dragstart="startDrag($event)"></block-lib-com>
    </div>
    <!-- <div @drop="drop($event)" @dragover.prevent @dragenter.prevent class="normal"></div>
    <div @drop="drop($event)" @dragover.prevent @dragenter.prevent class="small"></div> -->
    <div class="drops">
      <div class="relative z-0 " v-for="dropZone in dropZones" :key="dropZone.id" :id="'dropZone' + dropZone.id" :class="dropZone.type" @drop="drop($event), dropZone.hasBlock = true" @dragleave="dragLeave($event)" @dragover="dragOver($event)" @dragenter=dragEnter($event)></div>
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
        {type:'normal', id:1, hasBlock:false},
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
      evt.dataTransfer.setData('text/html', evt.target.id)
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
          let check = evt.target.id.replace(/\D/g,'')

          this.dropZones.forEach(zone=>{
            if(zone.id == check){
              zone.hasBlock = false
            }
          })
            if(this.dropZones.length == check ){
            this.dropZones = this.dropZones.filter(zone => zone.hasBlock == true)
          }
          })
          let nextDrop = this.dropZones.length
          const newDrop = {type: 'normal', id:nextDrop +1, hasBlock:false}
          this.dropZones.push(newDrop)
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
.blockLib{
  z-index: 5;
  width: 10%;
}
.home{
display: flex;
flex-direction: row-reverse;
width: 100vw;
overflow-x: hidden;
}
.drops{
  width: 91%;

}
.normal{
  width: 99%;
  height: 400px;
  max-height: fit-content;
  position: relative;
  transform: translateX(-10%);
  overflow-x: hidden;
}
.showDrop{
  border: dashed rgba($color: #000000, $alpha: 0.8) 1px;
}
.editBlock{
width: 100%;
}
</style>