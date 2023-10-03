<script setup>
const props = defineProps({
  data:{
    type:Object,
    required:true
  }
})
console.log(props.data);

</script>

<template>
  <div class=''>
   横向纵状图
  </div>
</template>

<style lang='scss' scoped>
</style>
