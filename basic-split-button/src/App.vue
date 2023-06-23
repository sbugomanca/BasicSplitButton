<script setup lang="ts">
import BasicSplitButton from './components/BasicSplitButton.vue'

let selected: string

let setSelectedValue = (val:string) => {
    selected = val 
    alert(selected)
  }

  let values = ["Test","Test2","Test3","Test4","Test5","Test6"]
  let btnType = "error"
</script>

<template>
  <BasicSplitButton @select="setSelectedValue" :options="values" :btnType="btnType.toLowerCase()"/>
</template>

<style scoped>
</style>
