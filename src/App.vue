<template>
 <div class="container" style="margin-top: 20px;">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">SIMPLE TODO APP</h5>
        <div class="row">
          <div class="col-10">
            <input v-model="todo" type="text" class="form-control" @keyup.enter="add">
          </div>
          <div class="col-2">
            <button class="btn btn-success" style="width: 100%;" @click="add">ADD</button>
          </div>
        </div>
        <list :todos="list" @deleteTodo="deleteTodo" @doneTodo="doneTodo"/>
        <small>Total Todo: {{ totalTodo }}</small>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, onMounted, computed, onBeforeMount, toRefs } from 'vue'
import List from './components/List.vue'
export default{
  components: { List }, 
  setup(){
    const todo = ref('')
    const todos = reactive({
      list: []
    })

    // onBeforeMount(() => {
	  //   if (JSON.parse(localStorage.getItem('todos'))) {
		//     return true
	  //   }
	  //   saveLocalStorage()
    // })

    onMounted(()=>{
      const items = localStorage.getItem('todos')
      todos.list = items ? JSON.parse(items) : []
    })

    const totalTodo = computed(() => {
      return todos.list.length
    })
     
    const add = () =>{
      todos.list.unshift({
          activity: todo.value,
          isDone: false
        })
      todo.value = ""
      saveToLocalStorage()
    }

    const deleteTodo = todoIndex => {
      todos.list = todos.list.filter((item, index) => {
        if(index != todoIndex){
            return item
          }
        })
      saveToLocalStorage()
    }
    
    const doneTodo = doneIndex => {
      todos.list = todos.list.filter((item, index) =>{
         if(index == doneIndex){
           item.isDone = !item.isDone;
         }

         return item
       })
      saveToLocalStorage()
    }

    const saveToLocalStorage = () =>{
      localStorage.setItem('todos', JSON.stringify(todos.list))
    }

    return {todo,...toRefs(todos),totalTodo,add,deleteTodo,doneTodo}
  }
}
</script>

