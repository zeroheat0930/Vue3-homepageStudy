<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import ChildComp from './ChildComp.vue'

// 예제 1
const titleClass = ref('title')

// 예제 2
const count = ref(0)
function increment() {
  count.value++
}

// 예제 3
const text = ref('')
// function onInput(e) {
//   text.value = e.target.value
// }

// 예제 4
const awesome = ref(true)
function toggle() {
  awesome.value = !awesome.value
}

// 예제 5
let id = 0
const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'HTML 배우기' },
  { id: id++, text: 'JavaScript 배우기' },
  { id: id++, text: 'Vue 배우기' }
])
function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}
function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
const filteredTodos = computed(() => {
  return hideCompleted.value
      ? todos.value.filter((t) => !t.done)
      : todos.value
})

// 예제 6
const pElementRef = ref(null)
onMounted(() => {
  pElementRef.value.textContent = '마운트 되었어요!'
})

// 예제 7
const todoId = ref(1)
const todoData = ref(null)
async function fetchData() {
  todoData.value = null
  const res = await fetch(
      `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  todoData.value = await res.json()
}
fetchData()
watch(todoId, fetchData)

// 예제 8
const greeting = ref('부모 컴포넌트로부터 💌을 전달받았어요!')

// 예제 9
const childMsg = ref('자식 컴포넌트로부터 아직 메시지를 받지 못했어요!')
</script>

<template>
  <h1 :class="titleClass">나를 빨갛게 만들어 보세요</h1>           <!-- 클래스 바인딩 :class="전역변수" -->

  <button @click="increment()">숫자 세기: {{ count }}</button>  <!-- 이벤트리스너 @기능="기능함수이름" -->

  <input v-model="text" placeholder="여기에 입력하기">           <!-- :value="text" @input="onInput" 해도 되지만 v-model로 바로 처리가능함 -->
  <p>{{ text }}</p>

  <button @click="toggle">토글 버튼</button>
  <h1 v-if="awesome">Vue는 굉장해! 엄청나!</h1>                  <!-- v-if, v-else로 분기처리 가능 -->
  <h1 v-else>오 안돼 😢</h1>

  <form @submit.prevent="addTodo">                             <!-- @submit.prevent를 사용하여 페이지가 새로고침 되거나 이동하지 않고, 디자인된 이벤트 핸들러만 실행되도록 만들 수 있습니다 -->
    <input v-model="newTodo">
    <button>할 일 추가</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">          <!-- v-for로 자료배열을 엘리먼트 목록으로 만듬 -->
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>

  <p ref="pElementRef">안녕</p>                                  <!-- 컴포넌트 생명 주기 훅 <- onMounted함수로 dom 조작 -->

  <p>할 일 id: {{ todoId }}</p>                                  <!-- watch기능인데 감시자, 이해가안됨 -->
  <button @click="todoId++">다음 할 일 가져오기</button>
  <p v-if="!todoData">로딩...</p>
  <pre v-else>{{ todoData }}</pre>

  <ChildComp :msg="greeting" />                                 <!-- 자식 컴포넌트 가져오기 -->

  <ChildComp @response="(msg) => childMsg = msg" />
  <p>{{ childMsg }}</p>
</template>


<style>
.title {
  color: red;
}
.done {
  text-decoration: line-through;
}
</style>
