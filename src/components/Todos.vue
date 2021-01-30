<template>
  <div id="Todos" class="bg-gray-200 h-screen font-sans">
    <div class="flex flex-col justify-center items-center ">
      <div>
        <h1 class="font-thin text-8xl py-6 text-gray-300">todos</h1>
      </div>
      <div
        class=" w-2/3 bg-white flex flex-col shadow-lg rounded-lg overflow-hidden"
      >
        <div id="add-todo" class="border-t-0 border-l-0 border-r-0 border-b-2">
          <div class="flex items-center mx-6 my-2">
            <button id="select-all" class="pt-4">
              <svg
                class=" w-8 h-8 text-gray-200 fill-current"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
                x="0px"
                y="0px"
                viewBox="0 0 404.257 404.257"
                style="enable-background:new 0 0 404.257 404.257;"
                xml:space="preserve"
              >
                <polygon
                  points="386.257,114.331 202.128,252.427 18,114.331 0,138.331 202.128,289.927 404.257,138.331 "
                />
              </svg>
            </button>

            <input
              type="text"
              class="border-0 outline-none text-3xl font-extralight ml-4 py-4 text-gray-600 placeholder- placeholder-gray-200 rounded-lg flex-grow"
              placeholder="What needs to be done?"
              autofocus
              autocomplete="off"
              v-model="newTodo"
              @keyup.enter="addTodo"
            />
          </div>
        </div>

        <div id="todo-list" v-show="todosShown.length">
          <div
            id="todo-item"
            class="flex items-center px-6 py-4 border-b-2"
            v-for="todo in filteredTodos"
            :key="todo.id"
          >
            <input
              id="complete-todo"
              type="checkbox"
              class="focus:shadow-none h-8 w-8"
              v-model="todo.completed"
            />

            <div
              id="todo-detail"
              class="flex flex-grow items-center justify-between"
            >
              <div
                v-if="todo.completed"
                id="todo-description"
                class="text-3xl font-extralight  text-gray-300 line-through px-6"
              >
                {{ todo.description }}
              </div>
              <div
                v-else
                id="todo-description"
                class="text-3xl font-extralight text-gray-400 px-6"
              >
                {{ todo.description }}
              </div>

              <button
                id="clear-todo"
                @click="removeTodo(todo)"
                class=" hover:text-red-500"
              >
                <svg
                  class=" w-8 h-8 text-yellow-200 fill-current"
                  fill="none"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path d="M6 18L18 6M6 6l12 12"></path>
                </svg>
              </button>
            </div>
          </div>
        </div>
        <div
          id="filter-todo"
          v-show="todos.length"
          class="grid grid-cols-5 place-items-center bg-gray-200 text-lg font-extralight text-gray-500 px-6 py-4"
        >
          <div class="col-span-1 w-full">
            <div class="flex text-left">{{ remainingTodos }} left</div>
          </div>
          <div class="col-span-3 flex flex-row flex-nowrap">
            <div v-for="view in viewOptions" v-bind:key="view.show">
              <div
                v-if="view.current"
                @click="changeView(view.show)"
                class="px-2 py-1 mx-2 rounded border border-gray-400"
              >
                {{ view.show }}
              </div>
              <div v-else @click="changeView(view.show)" class="px-2 py-1 mx-2">
                {{ view.show }}
              </div>
            </div>
          </div>

          <div class="flex truncate col-span-1 w-full text-right">
            <div v-show="todosCompleted > 0">Clear completed</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todos',
  data() {
    return {
      newTodo: '',
      todos: [],
      todosShown: 'All',
      viewOptions: [
        { show: 'All', current: true },
        { show: 'Active', current: false },
        { show: 'Completed', current: false }
      ]
    }
  },
  methods: {
    addTodo() {
      var value = this.newTodo && this.newTodo.trim()
      if (!value) {
        return
      }
      var todo = {
        id: this.todos.length,
        description: value,
        completed: false
      }
      this.todos.push(todo)
      this.newTodo = ''
    },
    changeView(selectedView) {
      for (let i = 0; i < this.viewOptions.length; i++) {
        if (this.viewOptions[i].show !== selectedView) {
          this.viewOptions[i].current = false
        } else {
          this.viewOptions[i].current = true
        }
        this.todosShown = selectedView
      }
    },
    removeTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1)
    }
  },
  computed: {
    filteredTodos() {
      if (this.todosShown === 'All') {
        return this.todos
      } else if (this.todosShown === 'Active') {
        return this.todos.filter(function(todo) {
          return !todo.completed
        })
      } else {
        //completed
        return this.todos.filter(function(todo) {
          return todo.completed
        })
      }
    },
    remainingTodos() {
      var remaining = this.todos.filter(function(todo) {
        return !todo.completed
      })
      return remaining.length
    },
    todosCompleted() {
      var completed = this.todos.filter(function(todo) {
        return todo.completed
      })
      return completed.length
    }
  }
}
</script>

<style lang="css"></style>
