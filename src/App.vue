<template lang="html">
  <div>
    <table>
      <thead>
        <tr>
          <th class="id">ID</th>
          <th class="comment">Comment</th>
          <th class="state">State</th>
          <th class="button">-</th>
        </tr>
      </thead>
        <tr v-for="item in $data.todos" v-bind:key="item.id">
          <th>{{ item.id }}</th>
          <td>{{ item.comment }}</td>
          <td class="state">
            <button>{{ item.state }}</button>
          </td>
          <td class="delete">
            <button>Delete</button>
          </td>
        </tr>
      <tbody>

      </tbody>
    </table>
  </div>
</template>

<script>
// use local storage
const STORAGE_KEY = 'todos-vusjs-demo';
const todoStorage = {
  fetch() {
    const todos = JSON.parse(
      localStorage.getItem(STORAGE_KEY) || '[]'
    );
    todos.forEach((todo, index) => {
      todo.id = index;
    });
    todoStorage.uid = todos.length;
  },
  save(todos) {
    localStorage.setItem(
      STORAGE_KEY, JSON.stringfy(todos));
  }
}

export default {
  name: 'App',
  data() {
    return {
      todos: []
    };
  }
}
</script>

<style lang="css" scoped>
</style>
