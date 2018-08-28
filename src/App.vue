<template>
  <div id="app">
    <div v-if="ready" class="blocks">
      <div v-for="(line, i) in matrix" :key="i" class="line">
        <block v-for="(el, j) in line" :data="el" :key="i+j" :onclick="activateBlock.bind(this, i, j)" />
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
      size: 4, // size of matrix
      activeBlocks: [],
      ready: false,
      timer: false,
      win: false,
    }
  },
  methods:{
    getColors(){ // generating array of random HEX colors
      let length = Math.floor(this.size*this.size/2);

      for(let i = 0; i < length; i++){
        this.colors.push(this.getRndColor())
      }

      this.colors = [...this.colors, ...this.colors]
      this.getMatrix();
    },
    getMatrix(){ // generating game matrix
      for(let i = 0; i < this.size; i++){
        this.matrix[i] = []
        for(let j = 0; j < this.size; j++){
          let color = this.addToMatrix();
          this.matrix[i].push({color: color, status: false, active: false});
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
          if(this.matrix[i][j].status === false){
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
</style>
