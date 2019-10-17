<template>
    <div class="wrapper">
        <div class="puzzle" :style="{width:blockWidth*row + 'px' ,height:blockHeight*col + 'px'}">
            <div class="puzzle-box" v-for="(item, index) in blockPosition" :key="item.key"  @click="haveClick"
            :style="{
            width:blockWidth + 'px',
            height:blockHeight + 'px',
            left : item.left + 'px',
            top : item.top + 'px',
            backgroundPosition: `${imgPosition[index].left}px ${imgPosition[index].top}px`,
            opacity: index === blockPosition.length-1 && 0}" :ref="index === blockPosition.length-1? 'empty' : 'block'"></div>
        </div>
        <button class="next" @click="next">下一关</button>
    </div>
    
</template>

<script>
export default {
    props : {
        row : {
            type : Number,
            default : 3
        },
        col : {
            type :Number,
            default : 3
        },
        blockWidth : {
            type : Number,
            default : 100
        },
        blockHeight : {
            type : Number,
            default : 100
        }
    },
    methods : {
      haveClick (e) {
        
        var clickBlock = e.target;
        var emptyBlock = this.$refs.empty[0];
        var left = clickBlock.style.left;
        var top = clickBlock.style.top;

        var numLeft = parseInt(left);
        var numTop = parseInt(top);
        var eNumLeft = parseInt(emptyBlock.style.left);
        var eNumTop = parseInt(emptyBlock.style.top);
        
        console.log(Math.abs(numLeft-eNumLeft),Math.abs(numTop-eNumTop));
        if(Math.abs(numLeft-eNumLeft)>=200 || Math.abs(numTop-eNumTop)>=200) {
          return;
        }
        if(Math.abs(numLeft-eNumLeft)>=100 && Math.abs(numTop-eNumTop)>=100) {
          return;
        }
        
        clickBlock.style.left = emptyBlock.style.left;
        clickBlock.style.top = emptyBlock.style.top;
        emptyBlock.style.left = left;
        emptyBlock.style.top = top;
      },
      next () {
        this.$emit("next");
      }
    },
    computed : {
        imgPosition () {
            var size = [];
            for(var i = 0;i < this.row ; i++) {
                for(var j=0;j < this.col ; j++) {
                  let key = Math.random()*100;
                    size.push({
                        left : -i*this.blockWidth,
                        top : -j*this.blockHeight,
                    });
                }
            }
            var last = size.pop();
            size.sort(function(a,b){ return Math.random()>.5 ? -1 : 1;});
            size.push(last);
            console.log(size);
            return size;
        },
        blockPosition () {
          var size = [];
          for(var i = 0;i < this.row ; i++) {
              for(var j=0;j < this.col ; j++) {
                let key = Math.random()*100;
                  size.push({
                      left : i*this.blockWidth,
                      top : j*this.blockHeight,
                      key : key
                  });
              }
          }
          
          return size;
        }
    }
}
</script>

<style>
.puzzle {
  position: relative;
}
.puzzle-box {
  position: absolute;
  box-sizing: border-box;
  background-clip: content-box;
  background : url(../assets/4.jpg);
}

</style>