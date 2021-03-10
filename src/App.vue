<template>
  <div class="flex flex-col items-center bg-gray-200">
    <h1 class="">Todo List</h1>
    <input type="text" class="form-control border border-black" v-model="userInput" @keyup.enter="addNewTodo">
    <div class="bg-gray-200 w-full text-center flex flex-col items-center">
    <template v-for="todo in activeTodoList" v-bind:key="todo.label">
      <button class="bg-green-200 w-6/12 text-left px-3 py-3"  @click="toggleTodoState(todo)">
        {{todo.label}}
      </button>
    </template>
    </div>
    <div class="text-right">
      <button class="" @click="chageCurrentState('active')">할 일</button>
      <button @click="chageCurrentState('done')">완료</button>
      <button @click="chageCurrentState('all')">전체</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'


export default defineComponent({
  name: 'App',
  data(){
    return{
      userInput: "",
      todoList: [] = [],
      currentState:'active'
    };
  },
  computed:{
    activeTodoList(){
      return this.todoList.filter((todo : any) => this.currentState === 'all' || todo.state === this.currentState);
    }
  },
  methods: {
    chageCurrentState(state:any){
        this.currentState = state;
    },
    addNewTodo(){
      this.todoList.push({
        label: this.userInput,
        state: 'active'
      });
      this.userInput = '';
    },
    toggleTodoState(todo:any){
      todo.state = todo.state === 'active' ? 'done' : 'active';
    }
  },
  components: {
 
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>