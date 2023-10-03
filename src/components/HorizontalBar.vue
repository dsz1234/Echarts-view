<script setup>
import {ref,onMounted,watch} from 'vue'
import * as echarts from 'echarts'
const props = defineProps({
  data:{
    type:Object,
    required:true
  }
})

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
      // 不显示x轴
      show:false,
      // 作为数据去展示
      type:'value',
      // 最大值以防止触顶 parseInt取整
      max:function(value){
        return parseInt(value.max * 1.2)
      }
    },
    // y轴展示数据
    yAxis:{
      // 作为类别展示
      type:'category',
      data:props.data.regions.map(item=>item.name),
      // 倒序展示数据
      inverse:true,
      // y轴不展示轴线
      axisLine:{
        show:false
      },
      // 不展示刻度
      axisTick:{
        show:false
      },
      // 文字颜色
      axisLabel:{
        color:'#9eb1c8'
      }
    },
    // 绘制图表的位置，对应 上下左右
    grid:{
      top:0,
      right:0,
      bottom:0,
      left:0,
      // 计算包含标签
      containLabel:true
    },
    // 核心配置
    series:[
      {
        // 柱形图
        type:'bar',
        data:props.data.regions.map(item=>({
          name:item.name,
          value:item.value
        })),
        // 背景
        showBackground:true,
        backgroundStyle:{
          color:'rgba(180,180,180,0.2)'
        },
        // 每个轴的样式
        itemStyle:{
          color:'#479AD3',
          borderRadius:8,
          shadowColor:'rgba(0,0,0,0.3)',
          shadowBlur:5
        },
        // 轴的宽度
        barWidth:12,
        // 轴上的字体
        label:{
          show:true,
          position:'right',
          color:'#fff'
        }
      }
    ]
  }
  // 3.通过实例.setOptions（option）
  mChart.setOption(options)
}

watch(()=>props.data,()=>{
  renderChart()
})


</script>

<template>
  <div class=''>
    <div>【大区数据信息】</div>
   <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<style lang='scss' scoped>
</style>
