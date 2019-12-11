<template>
  <div class="divide-bar">
    <div @click="select('before')" v-if="false">
      <i class="bar-up icon-chevron-up"></i>
    </div>
    <div class="divide-bar-stage" id="divide-bar-stage"
         :style="{ width: stageWidth + 'px' }" @click="handleEvent" @touchstart="handleEvent"
         @touchmove="handleEvent" @touchend="handleEvent">
      <div class="divide-bar-container" :class="{ easein: !isTouch }"
           :style="{ width: itemWidth + 'px', transform: `translateX(${realTranslate}px )` }">
<!--由于container包含了所有的item 让container移动可以使item整体实现移动
初始状态第一个div的中心轴线为原点
注意：鼠标移动方向正好和item移动方向相反，所有鼠标正向移动(clientX增大)时，item 整体反向移动(realTranslate<0)
-->
        <div class="divide-bar-item"
             :style="{ /*transform: `translateX(${translateRate * index}px)`*/
              width: itemWidth + 'px'}"    style="color: white"
             :class="{'active' : item === map, 'first-child': index === 0 }" v-for="(item, index) in maps" :key="index"
             @click="select(index)">
          <div v-show="item.dateDesc==='昨日'" class="divide">
            <div class="date" v-show="index===0">昨日</div>
            <div style="width: 76px;height: 52px;padding-top: 10px;
             background-image: url(https://s10.mogucdn.com/mlcdn/c45406/191210_59a75f4457jhief7681a6c82l3c47_750x110.jpg)">
              <p style="font-weight: bold;">{{item.timeDesc}}</p>
              <p style="font-size: 12px;margin-top: 5px">{{item.statusDesc}}</p>
            </div>
          </div>
        <div v-show="item.dateDesc==='今日'" class="divide">
          <div class="date" v-show="index===2">今日</div>
          <div style="width: 76px;height: 52px;padding-top: 10px;
             background-image: url(https://s10.mogucdn.com/mlcdn/c45406/191210_59a75f4457jhief7681a6c82l3c47_750x110.jpg)">
            <p style="font-weight: bold;">{{item.timeDesc}}</p>
            <p style="font-size: 12px;margin-top: 5px">{{item.statusDesc}}</p>
          </div>
        </div>
      <div v-show="item.dateDesc==='明日'" class="divide">
        <div class="date" v-show="index===9">明日</div>
        <div style="width: 76px;height: 52px;padding-top: 10px;
             background-image: url(https://s10.mogucdn.com/mlcdn/c45406/191210_59a75f4457jhief7681a6c82l3c47_750x110.jpg)">
          <p style="font-weight: bold;">{{item.timeDesc}}</p>
          <p style="font-size: 12px;margin-top: 5px">{{item.statusDesc}}</p>
        </div>
      </div>
    </div>

      </div>

    <div @click="select('next')" v-if="false">
      <i class="bar-down icon-chevron-down"></i>
    </div>
  </div>
  </div>
</template>

<script>
    export default {
        name: "Mobisroll2d",
        data() {
            return {
                stageWidth: 360,//滚动宽度
                itemWidth: 76,//每个div宽度
                translateRate: 76,//每次偏转位移
                itemNum: 18,//单圈最大容纳数目
                realTranslate: -456,//实际偏转位移
                lastTranslate: 0,
                eachOffset: 0,
                touchStart: 0,
                moveDistance: 0,
                isTouch: false
            };
        },
        props: {
            maps: {
                type: Array,
                default: function () {
                    return [];
                }
            },
            map: {
                type: Object,
                default: function () {
                    return {};
                }
            }
        },
        watch: {
            maps: function () {
                this.init();
            },
            map: function (val) {
                let thiz = this;
                this.maps.forEach(function (item, index) {
                    if (val === item) {
                        thiz.select(index);
                    }
                });
            }
        },
        beforeMount: function () {
            var styleNode = document.createElement('style');
            styleNode.setAttribute('id', this.$options._componentTag)
            // styleNode.innerText = style;
            document.body.appendChild(styleNode);
        },

        mounted: function () {
            let thiz = this;
            this.init();
            document.getElementById('divide-bar-stage');
            this.maps.forEach(function (item, index) {
                if (thiz.map === item) {
                    thiz.select(index);
                }
            });
        },
        beforeDestroy: function () {
            var styleNode = document.getElementById(this.$options._componentTag);
            document.body.removeChild(styleNode);
        },
        methods: {
            init: function () {
                console.log(this.itemWidth, this.translateRate);
            },
            select: function (index) {
                if (index > this.maps.length - 1) {
                    return;
                }
               // var r = this.translateRate * index + this.realTranslate;//需要偏转的位移
                this.$emit('update:map', this.maps[index]);
            },
            handleEvent: function (e) {
           //    console.log("clientX:"+e.touches[0].clientX);
                if (e.type === 'touchstart') {
                    this.touchStart = e.touches[0].clientX;
                    this.moveDistance = 0;
                    this.lastTranslate = this.realTranslate;
                    this.isTouch = true;
                }
                if (e.type === 'touchmove') {
                    e.preventDefault();
                    this.moveDistance = this.touchStart - e.touches[0].clientX;//鼠标和item反向移动
                    this.eachOffset = this.moveDistance;//偏转位移
                    this.realTranslate = this.lastTranslate + this.eachOffset;//修改真实位移
                }
                //下面判断的作用为限制偏转范围
                    if (this.realTranslate > this.translateRate / 2) {
                        this.realTranslate = this.translateRate / 2;
                    }

                    if (this.realTranslate < -this.translateRate * (this.maps.length - 1) - this.translateRate / 2) {
                        this.realTranslate = -this.translateRate * (this.maps.length - 1) - this.translateRate / 2;
                    }

                    if (e.type === 'touchend') {
                        var r = this.realTranslate % this.translateRate;//等分剩余位移
                        var c = this.realTranslate / this.translateRate;//等分数（小数）
                        c = parseInt(c);//等分数取整
                        var index;
                        if (this.realTranslate >= 0) {//向左偏转
                            if (r > this.translateRate / 2) {
                                this.realTranslate = this.realTranslate + this.translateRate - r;//去掉余数后加一，自动补一格
                                c++;
                            } else {//真实角度被20度等分后余角小于或等于一半
                                this.realTranslate = this.realTranslate - r;//直接去掉余数
                            }
                            console.log(this.realTranslate)
                            index = c;
                        } else {//向右偏转
                            if (r + this.translateRate / 2 < 0) {
                                this.realTranslate = this.realTranslate - this.translateRate - r;//滑动超过一半
                                c--;
                            } else {
                                this.realTranslate = this.realTranslate - r;
                            }
                            console.log(this.realTranslate)
                            index = Math.abs(c);
                        }
                        this.$emit('update:map', this.maps[index]);
                        this.isTouch = false;
                    }
                }

            }

    }
</script>

<style scoped>
  .divide-bar-stage {
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    background-color: deeppink;
  }
  .divide-bar-container {
    display: flex;
    flex-direction: row;
    position: relative;
    width: 520px;
    transform-style: flat;
  }
  .divide-bar-container.easein {
    transition: transform 0.5s ease;
  }
  .divide-bar-item {
    width: 100%;
    border-bottom: 1px solid #d3d3d3;
    text-align: center;
    color: #a4a4a4;
  }
  .divide-bar-item.active {
    color: #3a72ed;
  }
  .nav-item{
    width: 76px;
    height: 52px;
    overflow: hidden auto;
    list-style-type: none;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center
  }
  .divide{
    display: flex;
    align-items: center;
  }
</style>

