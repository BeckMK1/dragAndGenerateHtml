<template>
    <div class="block1 libBlock w-screen h-full bg-black ml-auto mr-auto drag" draggable="true" :id="id">
        <div class="blockNav hideNav" > 
          <i class="deleteBtn fa-solid fa-xmark"></i>
          <i class="upBtn fa-solid fa-angle-up"></i>
        </div>
        <div class="header w-full  h-6" :class="color"></div>
        <p >test test test test test</p>
        <div class="footer w-full bg-slate-800 h-6"></div>
    </div>
</template>

<script>
export default {
  props:{
    id:String,
    color:String
  },
  methods:{
  }
}
</script>

<style lang="scss" scoped>
.block1{
  cursor: grab;
  &:hover .showNav{
display: block;
}
}
.showNav{
display: none;
}
.libBlock{
width: 150px !important;
height: 100px !important;
position: relative;
margin-top: 16px;
}
.hideNav{
  display: none;
}
.blockNav{
  position: absolute;
  top:0;
  right: 0;
  i{
    background-color: black;
    padding: 5px;
    color:white;
    z-index: 99;
    position: relative;
  }
}
</style>