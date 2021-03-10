<template>
  <div class="container flex flex-col items-center bg-gray-200 m-auto p-5 rounded-3xl">
    <h1 class="text-3xl p-2">Todo List</h1>
    <input type="text" class="form-control border mb-2 rounded-md h-10 w-1/3" v-model="userInput" @keyup.enter="addNewTodo" placeholder=" 할 일 생성">
    <div class="w-full text-center flex flex-col items-center">
    <template v-for="todo in activeTodoList" v-bind:key="todo.label">
      <!--아래처럼 한줄에 v-for를 담아도 되고 위처럼 template로 분리해서 담아도 됨-->
      <!--<button class="bg-green-200 w-6/12 text-left px-3 py-3" v-for="todo in activeTodoList" v-bind:key="todo.label" @click="toggleTodoState(todo)">-->
      <!-- 
      <button class="bg-green-200 w-6/12 text-left p-3 border border-black" @click="toggleTodoState(todo)">
        {{todo.label}}
      </button>
      Todo 컴포넌트 적용으로 위 내용 삭제 -->
      <todo 
        :label="todo.label" 
        @componentClick="toggleTodoState(todo)"
      />
      <!-- @componentClick="toggleTodoState(todo)" 
      컴포넌트 내부에서 발생시킨 componentClick 이벤트를 받아 toggleTodoState함수를 실행
      -->
    </template>
    </div>
    <div class="flex items-center mt-2 mb-2">
      <button class="btn-primary m-1" @click="chageCurrentState('active')">할 일</button>
      <button class="btn-secondary m-1" @click="chageCurrentState('done')">완료</button>
      <button class="btn-success m-1" @click="chageCurrentState('all')">전체</button>
      <button class="btn-danger m-1" @click="resetTodoList">초기화</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Todo from './components/TodoVue.vue'


export default defineComponent({
  name: 'App',
  data(){
    return{
      userInput: "", //사용자 입력값을 받아올 변수 선언 //v-model로 연결
      todoList: [] = [], //사용자 입력값을 보관할 배열 생성
      currentState:'active' //출력할 상태값을 가진 변수 선언 //default값으로 'active'
    };
  },
  /* computed 속성 */
  //data 속성 값의 변화에 따라 자동으로 다시 연산
  //ex) computed 속성에서 사용하고 있는 data 속성 값이 변경되면서 전체 값을 다시 계산함

  /* computed 속성과 methods 속성의 차이점 */
  //methods 속성은 호출할 때만 해당 로직이 수행
  //computed 속성은 대상 data의 값이 변경되면 자동으로 수행
  //즉, 수동적으로 data를 갱신시키느냐, 능동적으로 data를 갱신시키느냐 그 차이
  computed:{
    //v-for="todo in activeTodoList"
    //computed안에 함수를 담으면 activeTodoList() 함수가 아닌 activeTodoList와 같이 변수처럼 사용 가능
  
    //todoList 배열에 필터(filter)를 걸고
    //currentState가 'all'이거나 currentState값과 같은 값인 todo만 배열에 담아 리턴하는 함수
    activeTodoList(){
      return this.todoList.filter((todo : any) => this.currentState === 'all' || todo.state === this.currentState);
    }
  },
  methods: {
    //@click="chageCurrentState('active')
    //state값을 받아 상태값을 변경하는 함수
    chageCurrentState(state:any){
        this.currentState = state;
    },
    //@keyup.enter="addNewTodo"
    //keyup 이벤트(키입력 이벤트)가 발생하고 입력값이 enter이면 addNewTodo()함수를 실행한다는 의미
    addNewTodo(){
      //this.todoList.push(this.userInput); (초기 ver, string형태) //아래처럼 json형태로 바꿔 state를 추가함
      this.todoList.push({
        label: this.userInput,  //json형태 갖추기 위해 label: this.userInput로 변경
        state: 'active'  //상태값을 가질 state 값 선언
      });
      //todoList에 userInput값을 추가한 뒤 userInput값은 ''으로 초기화 시켜주는 것
      this.userInput = '';
    },
    //@click="toggleTodoState(todo)
    //버튼 클릭시 state값을 변경하는 함수
    //todo 파라미터는 클릭한 항목을 받을 변수
    toggleTodoState(todo:any){
      todo.state = todo.state === 'active' ? 'done' : 'active';
    },

    //초기화
    resetTodoList(){
      this.todoList.length = 0;
    }
  },
  components: {
    Todo
  }
})
</script>

<style lang="postcss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.btn-primary {
  @apply py-1 px-3 bg-blue-600 hover:bg-blue-700 focus:ring-blue-500 focus:ring-offset-blue-200 text-white transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg;
}
.btn-secondary {
  @apply py-1 px-3 bg-gray-600 hover:bg-gray-700 focus:ring-gray-500 focus:ring-offset-gray-200 text-white transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg;
}
.btn-success {
  @apply py-1 px-3 bg-green-600 hover:bg-green-700 focus:ring-green-500 focus:ring-offset-green-200 text-white transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg;
}
.btn-danger {
  @apply py-1 px-3 bg-red-600 hover:bg-red-700 focus:ring-red-500 focus:ring-offset-red-200 text-white transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg;
}
.btn-warning {
  @apply py-1 px-3 bg-yellow-600 hover:bg-yellow-700 focus:ring-yellow-500 focus:ring-offset-yellow-200 text-white transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg;
}
.btn-info {
  @apply py-1 px-3 bg-purple-600 hover:bg-purple-700 focus:ring-purple-500 focus:ring-offset-purple-200 text-white transition ease-in duration-200 text-center text-base font-semibold shadow-md focus:outline-none focus:ring-2 focus:ring-offset-2 rounded-lg;
}
</style>