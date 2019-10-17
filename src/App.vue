<template>
  <div id="app">
    <puzzle @next="next" :row ='row' :col='col'></puzzle>
    <img src="./assets/4.jpg" alt="">
  </div>
</template>

<script>
import Puzzle from './components/Puzzle';
export default {
  name: 'app',
  data () {
    return {
      row : 3,
      col : 3,
      back : ['../assets/0.jpg','../assets/1.jpg','../assets/2.jpg']
    }
  },
  components: {
    Puzzle
  },
  methods : {
    next () {
      if(this.row>=5){
        return;
      }
      this.row++;
      this.col++;
    }
  }
}
</script>

<style lang="less">
#app {
  display: flex;
}
img {
  width: 200px;
  height: 200px;
  margin: 50px;
}
</style>
