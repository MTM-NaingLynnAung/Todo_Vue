<template>
  <div class="col-4 m-auto">
    <h1>Todo</h1>
    <div>
      <input type="text" class="form-control" v-model="newItem" @keyup.enter="addList" autofocus>
    </div>
    <ul class="mt-3 pt-3">
      <li class="d-flex align-items-center position-relative mb-4" v-for="(todo, index) in todos" :key="todo.id" :id="todo.complete ? 'done' : 'active'">
        <input type="checkbox" class="me-5 larger" v-model="todo.complete"> 
        <div @dblclick="edit(index)" @keyup.enter="removeInput(index)" class="w-100 d-flex align-items-center">
          <label :class="todo.complete ? 'line' : '' " class="w-100" style="text-align: left;">{{ todo.title }}</label>
          <span class="position-absolute size" @click="deleteItem(index)">&times;</span>
          <input :type="todo.type" id="edit" class="edit-form" v-model="todo.title" @blur="removeInput(index)" v-focus>
        </div>
      </li>
    </ul>
    
    <hr v-show="todos.length">
      <div class="d-flex justify-content-between align-items-center">
        <span class=""> {{ activeItem.length }} item left</span>
        <ul class="d-flex justify-content-between m-0 col-5" style="font-size:1rem">
          <li>
            <a href="#" @click="all">All</a>
          </li>
          <li>
            <a href="#active" @click="active">Active</a>
          </li>
          <li>
            <a href="#completed" @click="completed">Completed</a>
          </li>
        </ul>
        <span class="col-3"><a href="javascript:void(0)" @click="clear" v-show="completeItem.length > 0">Clear Completed</a></span>
      </div>

    
  </div>
</template>
<script>
export default {
  data() {
    return {
      todos: [],
      newItem: '',
    }
  },
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  methods: {
    addList(){
      let todo = { title: this.newItem, complete: false, type: 'hidden' }
      this.todos = [...this.todos,  todo]
      this.newItem = ''
    },
    deleteItem(index){
      this.todos.splice(index, 1)
    },
    edit(index){
      this.todos[index].type = 'text'
    },
    removeInput(index){
      this.todos[index].type = 'hidden'
    },
    all(){
      document.querySelectorAll("#active").forEach(e => {
        e.classList.remove('none')
      })
      document.querySelectorAll("#done").forEach(e => {
        e.classList.remove('none')
      })
    },
    active(){
      document.querySelectorAll("#done").forEach(e => {
        e.classList.add('none')
      })
      document.querySelectorAll("#active").forEach(e => {
        e.classList.remove('none')
      })
    },
    completed(){
      document.querySelectorAll("#active").forEach(e => {
        e.classList.add('none')
      })
      document.querySelectorAll("#done").forEach(e => {
        e.classList.remove('none')
      })
    },
    clear() {
      this.todos = this.activeItem
    }
  },
  computed: {
    activeItem() {
      return this.todos.filter(element => {
        return element.complete == false
      });
    },
    completeItem() {
      return this.todos.filter(element => {
        return element.complete == true
      })
    }
  },
  mounted() {  
    if (localStorage.getItem('todos')) {
      this.todos = JSON.parse(localStorage.getItem('todos'))
    }
  },
  watch: {
    todos: {
      handler() {
        localStorage.setItem('todos', JSON.stringify(this.todos))
      },
      deep: true
    }
  }
}
</script>
<style>
 ul{
  list-style: none;
  font-size: 20px;
 }
a{
  text-decoration: none!important;
  color: #2c3e50!important;
 }
 .size {
  right: 10px;
  font-size: 30px;
  cursor: pointer;
 }
 input.larger {
  width: 20px;
  height: 16px;
 }
 .line {
  text-decoration: line-through;
 }
 .edit-form {
    left: 53px;
    position: absolute;
    padding-left: 12px;
 }
 ul .none {
  display: none!important;
 }
</style>
