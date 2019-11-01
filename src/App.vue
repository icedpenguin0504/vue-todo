<template lang="html">
  <div class="container">
    <h1>ToDo app with Vue.js</h1>

    <!-- select condition -->
    <div class="select-form">
      <label class="condition"
        v-for="(label, index) in $data.options" v-bind:key="index">
        <input type="radio"
          v-model="current"
          v-bind:value="label.value">{{ label.label }}
      </label>
      <p>({{ computedTodos.length }} tasks)</p>
    </div>

    <!-- show todos -->
    <table>
      <thead>
        <tr>
          <th class="id">ID</th>
          <th class="comment">Comment</th>
          <th class="state">State</th>
          <th class="delete">-</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in computedTodos"
          v-bind:key="item.id"
          v-bind:class="{done: item.state}">
          <th>{{ item.id }}</th>
          <td>{{ item.comment }}</td>
          <th>
            <button v-on:click="doChangeState(item)">
              {{ item.state ? 'Done' : 'Doing' }}
            </button>
          </th>
          <th>
            <button v-on:click.shift="doRemove(item)">Delete</button>
          </th>
        </tr>
      </tbody>
    </table>
    <p>To delete a task, hold down the Shift key and press the 'Delete' button</p>

    <!-- create new todo -->
    <h2>Add new task</h2>
    <form class="add-form" v-on:submit.prevent="doAdd">
      Comment  <input type="text" ref="comment">
      <button type="submit">Add</button>
    </form>
  </div>
</template>

<script>
// Use local storage
const STORAGE_KEY = 'todos';
const todoStorage = {
  fetch() {
    const todos = JSON.parse(
      localStorage.getItem(STORAGE_KEY) || '[]'
    );
    todos.forEach((todo, index) => {
      todo.id = index;
    });
    todoStorage.uid = todos.length;
    return todos;
  },
  save(todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
  }
}

export default {
  name: 'App',
  data() {
    return {
      todos: [],
      options: [
        {value: -1, label: 'All'},
        {value: 0, label: 'Doing'},
        {value: 1, label: 'Done'}
      ],
      current: -1
    };
  },
  methods: {
    // Add new task to to-do list
    doAdd() {
      const comment = this.$refs.comment;
      if (!comment.value.length) {
        return;
      }
      this.$data.todos.push({
        id: todoStorage.uid++,
        comment: comment.value,
        state: 0
      });
      // Reset entered value
      comment.value = '';
    },
    doChangeState(item) {
      item.state = item.state ? 0 : 1;
    },
    doRemove(item) {
      const index = this.$data.todos.indexOf(item);
      this.$data.todos.splice(index, 1);
    }
  },
  computed: {
    computedTodos() {
      return this.$data.todos.filter((el) => {
        return this.$data.current === -1 ? true : this.$data.current === el.state;
      });
    }
  },
  watch: {
    todos: {
      handler(todos) {
        // Save todos to local storage automatically
        todoStorage.save(todos);

      },
      deep: true
    }
  },
  created() {
    this.todos = todoStorage.fetch();
  }
}
</script>

<style lang="scss">
$base-color: #0298E4;
$container-width: 650px;
$id-width: 45px;
$state-width: 60px;
$delete-width: 45px;

.container {
  font-family: Helvetica, sans-serif;
  width: $container-width;
  margin: 0 auto;
}

.select-form {
  margin: 50px 0 10px 0;

  .condition {
    margin-right: 20px;
  }

  p {
    display: inline-block;
  }
}


table {
  border-collapse: collapse;  // for setting border

  tr {
    height: 45px;
  }

  thead {
    color: $base-color;
    border-bottom: 2px solid $base-color;

    .id {
      width: $id-width;
    }
    .comment {
      width: calc(#{$container-width} - #{$id-width} - #{$state-width} - #{$delete-width});
    }
    .state {
      width: $state-width;
    }
    .delete {
      width: $delete-width;
    }
  }

  tbody {
    tr {
      border-bottom: 2px solid lightgray;
    }
    tr.done {
      background-color: #f5f5f5;
      color: lightgray;
    }
  }
}

input {
  outline: none;
}

button {
  background-color: $base-color;
  color: white;
  border-radius: 10px;
  font-size: 16px;
  outline: none;
}

.add-form {
  > button {
    margin-left: 10px;
  }
}
</style>
