<template>
  <button class="bg-green-200 w-6/12 text-left px-3 py-3" @click="clickButton">
        {{label}}
  </button>
</template>
<script>
export default {
  props:['label'],
  methods:{
    clickButton(){
      this.$emit('componentClick');
    }
  }
};
</script>