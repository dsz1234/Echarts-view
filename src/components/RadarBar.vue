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
  //  雷达的坐标系配置
  radar:{
    axisName:{
        color:'#05D5FF',
        fontSize:15
    },
    // 雷达绘制类型。polygon 多边形
    shape:'polygon',
    center:['50%','50%'],
    radius:'80%',
    // 开始的角度（可以避免绘制到边框之外）
    startAngle:120,
    // 轴线配置
    axisLine:{
      lineStyle:{
        color:'rgba(5,213,255,.8)'
      }
    },
    // 网格线
    splitLine:{
      show:true,
      lineStyle:{
        watch:1,
        color:'rgba(5,213,255,.8)'
      }
    },
    // 指示器文字
    indicator:props.data.risks.map(item=>({
      name:item.name,
      max:100
    })),
    // 设置不拆分区域
    splitArea:{
      show:false
    }
  },
  // 坐标极点
  polar:{
    center:['50%','50%'],
    radius:'0%',
  },
  // 坐标角度轴
  angleAxis:{
    min:0,
    // 分割间距
    interval:5,
    // 按照逆时针增长 默认为true顺时针
    clockwise:false,
    // 不显示坐标轴刻度
			axisTick: {
				show: false
			},
			// 不显示坐标轴文字
			axisLabel: {
				show: false
			},
			// 不显示坐标轴线
			axisLine: {
				show: false
			},
			// 不显示分割线
			splitLine: {
				show: false
			}
  },
  // 径向轴
  radiusAxis:{
    min:0,
    // 间隔
    interval:20,
    splitLine:{
      show:true
    }
  },
  // 图表核心配置
  series:{
    // 雷达图
    type:'radar',
    // 拐点的样式，还可以取值'rect','angle'等
    symbol:'circle',
    // 拐点的大小
    symbolSize:10,
    itemStyle:{
        color:'#05D5FF',
        opacity:0.5
    },
    // 区域填充样式
    areaStyle: {
				color: '#05D5FF',
				opacity: 0.5
		},
		// 线条样式
    lineStyle:{
      width:2,
      color:'#05D5FF'
    },
    label:{
        show:true,
        formatter: (params) => {
					return params.value
				},
        color:'#fff'
    },
    // 数据
    data:[
      {
        value:props.data.risks.map(item=>item.value)
      }
    ]
  }
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
    <div>【云端报警风险】</div>
   <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<style lang='scss' scoped>
</style>
