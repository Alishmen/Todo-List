<template>
  <div class="navv">
    <span :class="{'navs' : !view_all, 'navs-active' : view_all}" @click="selectNav(1)">VIEW ALL</span>
    <span :class="{'navs' : !completed, 'navs-active' : completed}" @click="selectNav(2)">COMPLETED</span>
    <span :class="{'navs' : !current, 'navs-active' : current}" @click="selectNav(3)">CURRENT</span>
    <span :class="{'navsPlus' : !addit, 'navs-activePlus' : addit}" @click="selectNav(4)">+</span>
  </div>

  <div v-if="view_all">
    <div class="content" v-for="todo in todos" :key="todo">
      <Todo :content="todo.content" :id="todo.id" :progress="todo.inProgress" @changedProgress="toggleCurrent" @remmed="removeTodo" @updatedContent="changeContent"/>
    </div>
  </div>
  <div v-if="completed">
    <div class="content" v-for="todo in Completed" :key="todo">
      <Todo :content="todo.content" :id="todo.id" :progress="todo.inProgress" @changedProgress="toggleCurrent" @remmed="removeTodo" @updatedContent="changeContent"/>
    </div>
  </div>
  <div v-if="current">
    <div class="content" v-for="todo in Current" :key="todo">
      <Todo :content="todo.content" :id="todo.id" :progress="todo.inProgress" @changedProgress="toggleCurrent" @remmed="removeTodo" @updatedContent="changeContent"/>
    </div>
  </div>
  <div v-if="addit">
    <NewTodo @addtodo="updateTodos"/>
  </div>
</template>

<script>
import { computed, ref } from 'vue'
import Todo from "../components/Todo.vue"
import NewTodo from "../components/NewTodo.vue"



export default {
  name: 'Home',
  components: {
    Todo, NewTodo
  },
  setup(){
    

    const view_all = ref(true)
    const completed = ref(false)
    const current = ref(false)
    const addit = ref(false)

    function selectNav(num){
      if(view_all.value && num != 1){
        view_all.value = false;
      }
      if(completed.value && num != 2){
        completed.value = false;
      }
      if(current.value && num != 3){
        current.value = false;
      }
      if(addit.value && num != 4){
        addit.value = false;
      }

      if(num == 1){
        view_all.value = true;
      }
      else if(num == 2){
        completed.value = true;
      }
      else if(num == 3){
        current.value = true;
      }
      else{
        addit.value = true;
      }
    }


    let todos = ref([
      {content: "This is a task", inProgress: true, id: 1},
      {content: "This is another task", inProgress: true, id: 2},
      {content: "This is yet another task", inProgress: false, id: 3},
      {content: "And one more task", inProgress: false, id: 4}
    ])

    const Completed = computed(() => {
      return todos.value.filter((todo) => todo.inProgress === false)
    })
    const Current = computed(() => {
      return todos.value.filter((todo) => todo.inProgress === true)
    })

    function toggleCurrent(theId){
      todos.value.forEach((todo) => {
        if(todo.id == theId){
          todo.inProgress = !todo.inProgress
        }
      })
    }
    
    function removeTodo(theId){
      let idx = 1
      todos.value = todos.value.filter((todo) => todo.id != theId)
      todos.value.forEach((todo) => {
        todo.id = idx;
        idx++
      }) 
    }

    function updateTodos(stuff){
      todos.value.push({content: stuff, inProgress: true, id: todos.value.length + 1})
    }

    function changeContent(theId, newContent){
      todos.value.forEach((todo) => {
        if(todo.id == theId){
          todo.content = newContent
        }
      })
    }

    return {view_all, completed, current, addit, selectNav, todos, Completed, Current, toggleCurrent, removeTodo, updateTodos, changeContent}
  }
}
</script>

<style scoped>
.navv{
  width: 800px;
  margin: 0 auto;
  border-radius: 20px;
  margin-top: 10px;
  text-align: center;
  background-color: rgb(170, 147, 147);
}
.navs{
  display: inline-block;
  margin: 25px;
  font-size: 0.85em;
  color: gray;
  transition: all 0.2s ease;
}
.navs-active{
  text-decoration: underline;
  text-decoration-color: coral;
  display: inline-block;
  margin: 25px;
  font-size: 0.9em;
  color: rgb(40, 40, 40);
  transition: all 0.2s ease;
}
.navs:hover, .navs-active:hover, .navsPlus:hover, .navs-activePlus:hover{
  cursor: pointer;
}
.content{
  text-align: center;
  margin: 20px auto;
}
.navsPlus{
  transform: translateY(10%);
  margin: 25px;
  display: inline-block;
  font-size: 1.5em;
  color: gray;
  transition: all 0.2s ease;
}
.navs-activePlus{
  transform: translateY(10%);
  display: inline-block;
  margin-left: 22.5px;
  margin-right: 22.5px;
  margin-top: 17.5px;
  margin-bottom: 17.5px;
  font-size: 2em;
  color: rgb(40, 40, 40);
  transition: all 0.2s ease;
}
</style>
