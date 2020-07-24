<template>
  <div class="vue-waterfall-photo">
    <template v-for="(pitem, pindex) in colList" >
      <div :id="'vue-waterfall-photo-item'+ pindex"  :style="vueWaterfallPhotoItem" class="vue-waterfall-photo-image-container" :key="'vue-waterfall-photo-item'+ pindex" >
        <template v-for="item in pitem" >
          <slot :item="item">
            <img :src="item.src" :key="item[dataKey.id]"></img>
          </slot>
        </template>
      </div>
    </template>
  </div>
</template>

<style>
  .vue-waterfall-photo-image-container img{
    height: auto;width: 100%;
  }

  .vue-waterfall-photo{
      display: -webkit-flex;
        display: flex;
        align-items:flex-start;
  }

  .vue-waterfall-photo-item {
    max-width: 33%;
    overflow: hidden !important;
  }
</style>

<script>

  export default {
    props: {
      lineGap: {
        required: false,
        default: 1
      },
      dataKey:{
            type: Object,
          default: () => {
          return {
            height:'height',
            width:'width',
            id:'id'
          }
        }
      },
      grow: {
        default: 2
      },
      initList: {
        default: () => ([])
      }
    },
    data: () => ({
      colList: [],
      colBottomHeight: [],
    }),
    created() {
      this.refresh();
    },
    watch: {
      initList:function(newQuestion, oldQuestion){
        this.refresh();
      }
    },
    computed: {
      vueWaterfallPhotoItem: function() {
        return {
          'width': this.itemWidth,
          float: 'left',
          overflow: 'hidden',
           margin: this.lineGap+"%"
        }
      },
      itemWidth:function(){
        return 1 / this.grow  *100 +"%";
      },
      //item的真实宽度
      itemTrueWidth:function(){
        //减去0.5边距
        let countWidth= document.body.scrollWidth-(document.body.scrollWidth* 0.005 *this.grow*2);
        let width = countWidth / this.grow;
        return width;
      }
    },
    methods: {
      addItem(item) {
        var mixIndex = this.getMixIndex();
        this.colList[mixIndex].push(item);

        // var heightPercent = this.$refs['vue-waterfall-photo-item'+ mixIndex].offsetHeight;
        var ele = document.getElementById('vue-waterfall-photo-item'+ mixIndex);

        var heightPercent = item[this.dataKey.height] * ( this.itemTrueWidth / item[this.dataKey.width] );
        this.colBottomHeight[mixIndex] = this.colBottomHeight[mixIndex] + heightPercent;
      },
      joinItem(list) {
        for (var i = 0; i < list.length; i++) {
          this.addItem(list[i]);
        }
      },
      refresh() {
        this.colList = [];
        this.colBottomHeight = [];
        //初始列
        for (var i = 0; i < this.grow; i++) {
          this.colList.push([]);
          this.colBottomHeight.push(0);
        }
        this.joinItem(this.initList);
      },
      getMixIndex() {
        var mixIndex = this.colBottomHeight.length-1;
        var mixNum = this.colBottomHeight[this.colBottomHeight.length-1];
        for (var i = this.colBottomHeight.length-2; i >=0; i--) {
          if (this.colBottomHeight[i] <= mixNum) {
            mixNum = this.colBottomHeight[i];
            mixIndex = i;
          }
        }
        console.log(this.colBottomHeight)

        return mixIndex;
      }
    },
  }
</script>
