<template>
  <section class="real-app">
    <input
      type="text"
      class="add-input"
      autofocus="autofocus"
      placeholder="接下去要做什么？"
      @keyup.enter="addTodo"
    >
    <Item 
      :todo="todo"
      v-for="todo in filterTodo"
      :key="todo.id"
      @del="deleteTodo"
    />
    <Tabs :filter="filter"
          :todos="todos"
          @toggle="tab"
          @clearAll="clearAll"
    >

    </Tabs>
  </section>
</template>
<script>
import Item from './item.vue'
import Tabs from './tabs.vue'
let id = 0
export default {
   components: {
     Item,
     Tabs
   },
    data() {
    return {
      todos: [],
      filter: 'all'
    }
  },
  methods: {
      addTodo (e) {
        this.todos.unshift({
            id: id++,
            content: e.target.value,
            complated: false
        })
        e.target.value = ''
      },
      deleteTodo (id) {
        this.todos.splice(this.todos.findIndex(todo => {todo.id == id}),1)
      },
      tab (state) {
          this.filter = state
      },
      clearAll () {
        this.todos = this.todos.filter(todo => !todo.complated)
      }
  },
  computed: {
    filterTodo () {
      if(this.filter === 'all'){
        return this.todos;
      }else if(this.filter === 'active'){
        return this.todos.filter(todo => todo.complated == false)
      }else if(this.filter === 'complated'){
        return this.todos.filter(todo => todo.complated == true)
      }
    }
  }
}
</script>
<style lang="less" scoped>
.real-app{
  width: 600px;
  margin: 0 auto;
  box-shadow: 0 0 5px #666
}
.add-input{
  position: relative;
  margin: 0;
  width: 100%;
  font-size: 24px;
  font-family: inherit;
  font-weight: inherit;
  line-height: 1.4em;
  border: 0;
  outline: none;
  color: inherit;
  padding: 6px;
  border: 1px solid #999;
  box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
  box-sizing: border-box;
  font-smoothing: antialiased;
  padding: 16px 16px 16px 60px;
  border: none;
  box-shadow: inset 0 -2px 1px rgba(0,0,0,0.03);
}
</style>
