<template>
  <div id="app" class="container my-3">
    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text" id="basic-addon1">待辦事項</span>
      </div>
      <input
        type="text"
        class="form-control"
        placeholder="準備要做的任務"
        v-model="newTodo"
        @keyup.enter="addTodo"
      />
      <div class="input-group-append">
        <button class="btn btn-primary" type="button" @click="addTodo">
          新增
        </button>
      </div>
    </div>
    <div class="card text-center">
      <div class="card-header">
        <ul class="nav nav-tabs card-header-tabs">
          <li class="nav-item">
            <a
              class="nav-link"
              :class="{ active: filter == 'all' }"
              @click="filter = 'all'"
              href="#"
              >全部</a
            >
          </li>
          <li class="nav-item">
            <a
              class="nav-link "
              :class="{ active: filter == 'active' }"
              @click="filter = 'active'"
              href="#"
              >進行中</a
            >
          </li>
          <li class="nav-item">
            <a
              class="nav-link"
              :class="{ active: filter == 'complete' }"
              @click="filter = 'complete'"
              href="#"
              >已完成</a
            >
          </li>
        </ul>
      </div>
      <ul class="list-group list-group-flush text-left">
        <li
          @dbclick="editTodo(item)"
          class="list-group-item"
          v-for="(item, idx) in filterTodos"
          :key="item.id"
        >
          <div class="d-flex" v-if="item.id != cacheTodo.id">
            <div class="form-check">
              <input
                type="checkbox"
                class="form-check-input"
                :id="item.id"
                v-model="item.completed"
              />
              <label
                class="form-check-label"
                :class="{ completed: item.completed }"
                for="item.id"
              >
                {{ item.title }}
              </label>
            </div>
            <button
              type="button"
              class="close ml-auto"
              @click="removeTodo(idx)"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
            <input v-model="cacheTitle" v-if="item.id === cacheTodo.id" type="text" class="form-control" />
          </div>
        </li>
        <!-- <li class="list-group-item">
          <input type="text" class="form-control" />
        </li> -->
      </ul>
      <div class="card-footer d-flex justify-content-between">
        <span>還有 3 筆任務未完成</span>
        <a href="#">清除所有任務</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      newTodo: '',
      todos: [
        {
          id: '123',
          title: '尼好',
          completed: 'false'
        }
      ],
      filter: 'all',
      cacheTodo: {},
      cacheTitle: ''
    }
  },
  methods: {
    addTodo () {
      // 清除空白
      const value = this.newTodo.trim()
      // 隨機時間
      const timestamp = Math.floor(Date.now())
      if (!value) {
        return
      }
      this.todos.push({
        id: timestamp,
        title: value,
        completed: false
      })
      this.newTodo = ''
    },
    // 傳入idx
    removeTodo (key) {
      this.todos.splice(key, 1)
    },
    editTodo (item) {
      this.cacheTodo = item
      this.cacheTitle = item.title
    }
  },
  computed: {
    filterTodos () {
      if (this.filter === 'all') {
        return this.todos
      } else if (this.filter === 'active') {
        const newtodos = []
        this.todos.forEach(item => {
          if (!item.completed) {
            newtodos.push(item)
          }
        })
        return newtodos
      } else if (this.filter === 'complete') {
        const newtodos = []
        this.todos.forEach(item => {
          if (item.completed) {
            newtodos.push(item)
          }
        })
        return newtodos
      }
      return []
    }
  }
}
</script>

<style lang="scss">
.completed {
  text-decoration: line-through;
}
</style>
