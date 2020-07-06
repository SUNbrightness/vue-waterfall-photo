<template>
  <div class="vue-waterfall-photo">
    <template v-for="(pitem, pindex) in colList">
      <div :style="vueWaterfallPhotoItem" class="vue-waterfall-photo-image-container">
        <template v-for="item in pitem">
          <slot :item="item">
            <img :src="item.src"></img>
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
  }

  .vue-waterfall-photo-item {
    max-width: 30%;
    float: left;
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
          'max-width': this.itemWidth,
          float: 'left',
          overflow: 'hidden',
           margin: this.lineGap+"%"
        }
      },
      itemWidth:function(){
        return 1 / this.grow  *100 +"%";
      }
    },
    methods: {
      addItem(item) {
        var mixIndex = this.getMixIndex();
        this.colList[mixIndex].push(item);

        var heightPercent = item[this.dataKey.height] / item[this.dataKey.width];

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
        var mixIndex = 0;
        var mixNum = this.colBottomHeight[0];
        for (var i = 1; i < this.colBottomHeight.length; i++) {
          if (this.colBottomHeight[i] <= mixNum) {
            mixNum = this.colBottomHeight[i];
            mixIndex = i;
          }
        }

        return mixIndex;
      }
    },
  }
</script>
