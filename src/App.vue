<template>
  <div id="app">
    <block/>
  </div>
</template>

<script>
import block from './components/Block.vue'

export default {
  name: 'app',
  data: function(){
    return {
      hello: "Hello world",
      colors: [],
      matrix: [],
      size: 4,
    }
  },
  methods:{
    getColors(){
      let length = Math.floor(this.size*this.size/2);

      for(let i = 0; i < length; i++){
        this.colors.push(this.getRndColor())
      }
      this.colors = [...this.colors, ...this.colors]
      this.getMatrix();
    },
    getMatrix(){
      for(let i = 0; i < this.size; i++){
        this.matrix[i] = []
        for(let j = 0; j < this.size; j++){
          let color = this.addToMatrix();
          this.matrix[i].push([color]);
        }
      }
    },
    getRndColor(){
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
</style>
