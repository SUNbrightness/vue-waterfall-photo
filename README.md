# vue-waterfall-photo

## show
![show](https://raw.githubusercontent.com/SUNbrightness/my-img/master/WeChatdf46e4c89629031d2e0e4da40bd81041.png)


## Project setup
```
npm install vue-waterfall-photo -S
```
```
import VueWaterfallPhoto from 'vue-waterfall-photo';
    export default {
        name: "app",
        components: {
            VueWaterfallPhoto
        }
    }
```

## Simple

```
<template>
    <div >
            <vue-waterfall-photo :lineGap="0.5" :grow="3" :initList="imageList">
            </vue-waterfall-photo>
    </div>
</template>
<script>
    import VueWaterfallPhoto from 'vue-waterfall-photo';
    export default {
        name: "app",
        components: {
            VueWaterfallPhoto
        },
        data:function(){
            return {
                imageList:[
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic.jpg",height:300,width:300},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic0.jpg",height:300,width:300},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic1.jpg",height:100,width:100},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic2.jpg",height:100,width:100},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic7.jpg",height:1188,width:1200},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic4.jpg",height:300,width:300},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic5.jpg",height:300,width:300},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic6.jpg",height:300,width:300},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic7.jpg",height:1188,width:1200},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic8.jpg",height:300,width:300},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic9.jpg",height:2315,width:2315},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pavlova.jpg",height:853,width:1280},
                    {src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/test/pic.jpg",height:300,width:300},
                    
                ]
            }
        }
    };
</script>
```

## slot
```
  <vue-waterfall-photo ref="waterfall" :lineGap="0.5" :grow="3" :initList="imageList">
                 <template v-slot="{ item }">
                        <img  :src="'http://'+item.src"></img>
                    </template>
            </vue-waterfall-photo>

```
## performance
```
Don’t change imageList


use
this.$ref.waterfall.addItem({})
this.$ref.waterfall.joinItem([])
```

## initList
```
{
    src:"https://raw.githubusercontent.com/SUNbrightness/my-img/master/WeChatdf46e4c89629031d2e0e4da40bd81041.png",
    height:300,
    width:300,
}
```

## prop
required|Attribute|type| default |explain
---|---|---|---|---
true|initList|Array|[]|初始值
flase|lineGap|Number <100 |1|图片间隔距离
flase|grow|Number|2|列数
flase|dataKey|Object|{height:'height',width:'width',id:'id'}|属性映射

## methods
method|explain
---|---
addItem({})|新增单个数据
joinItem([])|新增一组数据
refresh()|初始化


### If don’t use add method, It’s also true.
### use add method is improve performance.