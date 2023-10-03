<script setup>
import {ref,onMounted,watch} from 'vue'
import * as echarts from 'echarts'
const props = defineProps({
  data:{
    type:Object,
    required:true
  }
})
// console.log(props.data);

// 1.初始化echarts实例
let mChart = null
const target = ref(null)
onMounted(()=>{
  mChart =  echarts.init(target.value)
  renderChart()
})
// 2.构建option 配置对象
const renderChart = ()=>{
  const options = {
    // x轴展示数据
    xAxis:{
      // 作为数据去展示
      type:'category',
      data:props.data.servers.map(item=>item.name),
      axisLabel:{
        color:'#9EB1C8'
      }
    },
    // y轴展示数据
    yAxis:{
      show:false,
      // 作为类别展示
      type:'value',
      max:function(value){
        return parseInt(value.max * 1.2)
      }
    },
    // 绘制图表的位置，对应 上下左右
    grid:{
      top:16,
      right:0,
      bottom:26,
      left:-26,
      // 计算包含标签
      containLabel:true
    },
    // 核心配置
    series:[
      {
        // 柱形图
        type:'bar',
        data:props.data.servers.map(item=>({
          name:item.name,
          value:item.value
        })),
        // 每个轴的样式
        itemStyle:{
          color:'#479AD3',
          borderRadius:5,
          shadowColor:'rgba(0,0,0,0.3)',
          shadowBlur:5
        },
        // 轴的宽度
        barWidth:12,
        // 轴上的字体
        label:{
          show:true,
          position:'top',
          color:'#fff',
          formatter:'{c}%'
        }
      }
    ]
  }
  // 3.通过实例.setOptions（option）
  mChart.setOption(options)
}
watch(()=>props.data,renderChart)
</script>

<template>
  <div class=''>
   <div>【服务资源占用比】</div>
   <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<style lang='scss' scoped>
</style>
