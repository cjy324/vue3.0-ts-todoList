<template>

  <button class="bg-white w-6/12 text-left p-2 mb-1 mt-1 rounded-md hover:bg-green-200" @click="clickButton">
        {{ label }}
  </button>

</template>


<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props:{
    label:{
      type: String,
      required: true
    }
    },
    //해당 컴포넌트에서 발생하는 이벤트를 외부에 알려줘야 함
    //@click="clickButton" //버튼 클릭시 clickButton() 함수 실행
  methods:{
    clickButton(){
      this.$emit('componentClick');
    }
  },
  name: 'Todo'
});
</script>