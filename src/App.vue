<template>
  <div id="app">
    <div v-if="win" class="message"><span class="message__text">You won! <button @click = "restart()">Restart</button></span></div>
    <div class="toolbar">Size: <input v-model.number="size"/> <button @click="restart()">Restart</button></div>
    <div v-if="ready" class="blocks">
      <div v-for="(line, i) in matrix" :key="i" class="line">
        <block v-for="(el, j) in line" :size="typeof size == 'number' ? size : 0" :data="el" :key="i+j" :onclick="activateBlock.bind(this, i, j)" />
      </div>
    </div>
  </div>
</template>

<script>
import block from './components/Block.vue'

export default {
  name: 'app',
  data: function(){
    return {
      colors: [],
      matrix: [],
      size: 5, // size of matrix
      activeBlocks: [],
      ready: false,
      timer: false,
      win: false,
    }
  },
  methods:{
    getColors(){ // generating array of random HEX colors
      this.ready = false;
      let length = Math.floor(this.size*this.size/2);

      for(let i = 0; i < length; i++){
        this.colors.push(this.getRndColor())
      }

      this.colors = [...this.colors, ...this.colors]
      this.getMatrix();
    },
    getMatrix(){ // generating game matrix
      if(this.size%2){
        var center = Math.floor(this.size/2)
      }
      for(let i = 0; i < this.size; i++){
        this.matrix[i] = []
        for(let j = 0; j < this.size; j++){
          
          if(this.size%2 && i === center && j === center){
            this.matrix[i].push({color: '#111', status: false, active: false, prop: true});
          }
          else{
            let color = this.addToMatrix();
            this.matrix[i].push({color: color, status: false, active: false, prop: false});
            }
        }
      }

      this.ready = true;
    },
    getRndColor(){ // generating color
      let color = "";
      let possible = "ABCDEF0123456789";

      for (let i = 0; i < 6; i++){
        color += possible.charAt(Math.floor(Math.random() * possible.length));
      }
      return color;
    },
    addToMatrix(){
      let colorId = Math.floor(Math.random() * this.colors.length);
      let color = this.colors[colorId];
      this.colors.splice(colorId, 1);
      return color;
    },
    activateBlock(i, j){ // block onclick
      if(this.activeBlocks[0]){
        if(this.activeBlocks[0].i === i && this.activeBlocks[0].j === j){
          //..
        }else{
          this.activate(i, j)
        }
      }else{
        this.activate(i, j)
      }
      
    },
    activate(i, j){
      if(!this.matrix[i][j].status && !this.timer){
        this.matrix[i][j].active = true;
        this.activeBlocks.push({i: i, j: j});
        this.updateMatrix();
        this.checkBlocks();
      }
    },
    checkBlocks(){ // check game end
      if(this.activeBlocks.length > 1 && !this.timer){
        if(this.matrix[this.activeBlocks[0].i][this.activeBlocks[0].j].color == this.matrix[this.activeBlocks[1].i][this.activeBlocks[1].j].color){
          this.matrix[this.activeBlocks[0].i][this.activeBlocks[0].j].status = true;
          this.matrix[this.activeBlocks[1].i][this.activeBlocks[1].j].status = true;
          this.activeBlocks = [];
          this.updateMatrix();
        }else{
          this.deactivateBlocks();
        }
      }
      let didWin = true;
      for(let i = 0; i < this.matrix.length; i++){
        for(let j = 0; j < this.matrix[i].length; j++){
          if(this.matrix[i][j].status === false && !this.matrix[i][j].prop){
            didWin = false;
            break;
          }
        }
      }
      this.win = didWin;
    },
    deactivateBlocks(){ //deactivating different blocks
      this.timer = true;
      setTimeout(()=>{
        for(let i = 0; i < this.matrix.length; i++){
          for(let j = 0; j < this.matrix[i].length; j++){
            if(this.matrix[i][j].status === false && this.matrix[i][j].active === true){
                this.matrix[i][j].active = false;       
            }
          }
        }
        this.updateMatrix();
        this.timer = false;
        this.activeBlocks = [];
      }, 1000)
    },
    updateMatrix(){
      this.matrix = JSON.parse(JSON.stringify(this.matrix));
    },
    restart(){
      this.colors = [];
      this.matrix = [];
      this.activeBlocks = [];
      this.timer = false;
      this.win = false;
      this.getColors();
    }
  },
  created: function(){
    this.getColors();
  },
  components: {
    block
  },
  
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
}
.line{
  display: flex;
  flex-direction: row;
  justify-content: center;
  
}
.message{
  position: fixed;
  width: 300px;
  height: 140px;
  text-align: center;
  left: calc(50vw - 150px);
  top: calc(30vh - 70px);
  background: #eee;
  box-shadow: #777 0px 0px 1px;
  z-index: 2;
}
.message__text{
  position: relative;
  top: calc(50% - 20px);
  font-size: 20px;
}
.toolbar{
  text-align: center;
  margin-bottom: 30px;
}
</style>
