<template>
  <amplify-authenticator>
    <h1>Hello Benjamin</h1>
    <p>this is a todo app</p>
    <div class="addArea">
      <input type="text" v-model="name">
      <input type="text" v-model="discription">
      <button class="addButton" @click="createTodo">追加</button>
      <div>番号 タスク</div>
      <ul>
        <li v-for="todo in todos" v-bind:key="todo.id">
          {{todo.name}} ：{{todo.discription}}
          <!-- <input type="checkbox" v-model="item.state" /> -->
          <!-- <button @click="doRemove(index)">タスクを消去</button> -->
        </li>
      </ul>
    </div>
    <amplify-sign-out></amplify-sign-out>
  </amplify-authenticator>
</template>

<script>
import { API } from 'aws-amplify'
import { createTodo } from '~/src/graphql/mutations'
import { listTodos } from '~/src/graphql/queries'
export default {
  data() {
    return {
      name :'',
      discription: 'タスクを入力してください',
      // タスクインスタンス(stateは完了・未完了をboolで表現)
      todos: [],
    }
  },
  async created() {
    await this.getTodos()
  },
  methods: {
    async createTodo() {
      const { name, description } = this
      if (!name || !description) return false
      const todo = { name, description }
      await API.graphql({
        query: createTodo,
        variables: { input: todo },
      })
      this.name = ''
      this.description = ''
      console.log("created todo");
    },
    async getTodos() {
      const todos = await API.graphql({
        query: listTodos,
      })
      this.todos = todos.data.listTodos.items
    },
    subscribe() {
      API.graphql({ query: onCreateTodo }).subscribe({
        next: (eventData) => {
          const todo = eventData.value.data.onCreateTodo
          if (this.todos.some((item) => item.name === todo.name)) return // remove duplications
          this.todos = [...this.todos, todo]
        },
      })
    },
  }
};
</script>
