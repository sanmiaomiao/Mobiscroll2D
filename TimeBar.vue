<template>
    <div class="time_bar">
      <div class="nav-wrap">
        <div  id='nav-active-mask' class="nav-active-mask" style="background: rgb(255, 203, 221)"></div>
          <mobisroll2d :map.sync="map" :maps="maps"/>
        <!--修饰符sync:它会被扩展为一个自动更新父组件属性的 v-on 监听器
        vue 修饰符sync的功能是：当一个子组件改变了一个 prop 的值时，这个变化也会同步到父组件中所绑定
        -->
        <div>selected：{{ map }}</div>

      </div>
    </div>
</template>

<script>
 import Mobisroll2d from "./Mobisroll2d";
    export default {
        name: "TimeBar",
        components: {Mobisroll2d},
        data(){
            return{
                maps: [],
                map: {},//selected item
                nav:[{"statusDesc":"已开抢","timeDesc":"21:00","leftTime":0,"startTime":1575896400,"dateDesc":"昨日","selected":false,"status":3},
                    {"statusDesc":"已开抢","timeDesc":"22:00","leftTime":0,"startTime":1575900000,"dateDesc":"昨日","selected":false,"status":3},
                    {"statusDesc":"新品首发","acm":"3.mce.1_10_1nad2.27637.0.h9qFsrKfpUjli.pos_1-m_543231-sd_119","dateDesc":"今日","liveActivityId":0,"special":true,"timeMark":"4","timeDesc":"00:00","bannerLink":"","leftTime":0,"startTime":1575907200,"bannerImg":"","selected":false,"status":3},
                    {"statusDesc":"已开抢","timeDesc":"10:00","leftTime":0,"startTime":1575943200,"dateDesc":"今日","selected":false,"status":3},
                    {"statusDesc":"已开抢","timeDesc":"13:00","leftTime":0,"startTime":1575954000,"dateDesc":"今日","selected":false,"status":3},
                    {"statusDesc":"已开抢","timeDesc":"16:00","leftTime":0,"startTime":1575964800,"dateDesc":"今日","selected":false,"status":3},
                    {"statusDesc":"主推爆款","acm":"3.mce.1_10_1nacw.27637.0.h9qFsrKfpUjlj.pos_2-m_543228-sd_119","dateDesc":"今日","liveActivityId":0,"special":true,"timeMark":"4","timeDesc":"19:00","bannerLink":"","leftTime":6504,"startTime":1575975600,"bannerImg":"","selected":true,"status":2},
                    {"statusDesc":"即将开抢","timeDesc":"21:00","leftTime":6504,"startTime":1575982800,"dateDesc":"今日","selected":false,"status":1},
                    {"statusDesc":"即将开抢","timeDesc":"22:00","leftTime":10104,"startTime":1575986400,"dateDesc":"今日","selected":false,"status":1},
                    {"statusDesc":"新品首发","acm":"3.mce.1_10_1nad4.27637.0.h9qFsrKfpUjlk.pos_3-m_543232-sd_119","dateDesc":"明日","liveActivityId":0,"special":true,"timeMark":"4","timeDesc":"00:00","bannerLink":"","leftTime":17304,"startTime":1575993600,"bannerImg":"","selected":false,"status":1},
                    {"statusDesc":"即将开抢","timeDesc":"10:00","leftTime":53304,"startTime":1576029600,"dateDesc":"明日","selected":false,"status":1},
                    {"statusDesc":"即将开抢","timeDesc":"13:00","leftTime":64104,"startTime":1576040400,"dateDesc":"明日","selected":false,"status":1},
                    {"statusDesc":"即将开抢","timeDesc":"16:00","leftTime":74904,"startTime":1576051200,"dateDesc":"明日","selected":false,"status":1},
                    {"statusDesc":"热门推荐","acm":"3.mce.1_10_1nad0.27637.0.h9qFsrKfpUjll.pos_4-m_543230-sd_119","dateDesc":"明日","liveActivityId":0,"special":true,"timeMark":"4","timeDesc":"19:00","bannerLink":"","leftTime":85704,"startTime":1576062000,"bannerImg":"","selected":false,"status":1}]
            }

        },
        mounted() {

            console.log('mounted')
            console.log('mounted')
            console.log(this.nav)
          this.maps =this.nav;
            this.map = this.nav[6];//init first selected item*/

        },
        methods:{
            getmap(){
                console.log('getmap')
               // alert(this.nav)
                this.$emit('update:nav',false);
                this.maps =this.nav;
                this.map = this.nav[6]
            }
        },
        watch:{
            map:function (val) {
                console.log(val)
            }
        }
    }
</script>

<style scoped>
  .nav-wrap{
    position: relative;
    height: 52px;
  }
.nav-active-mask{
  position: absolute;
  width: 76px;
  height:52px;
  top: 0;
  left: 50%;
  opacity: 0.5;
  transform: translateX(-50%);
  z-index: 100;
}
 #nav-active-mask::after{
    content: "";
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    height: 0;
    width: 0;
    margin: auto;
    border-top: 5px solid #ff5777;
    border-left: 5px solid transparent;
    border-right: 5px solid transparent;
  }


</style>
