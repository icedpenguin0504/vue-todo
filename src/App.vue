<template lang="html">
  <div class="container">
    <h1>ToDo app with Vue.js</h1>
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
          v-for="item in $data.todos"
          v-bind:key="item.id"
          v-bind:class="{done: item.isDone}">
          <th>{{ item.id }}</th>
          <td>{{ item.comment }}</td>
          <th>
            <button v-on:click="doChangeState(item)">
              {{ item.isDone ? 'Done' : 'Doing' }}
            </button>
          </th>
          <th>
            <button>Delete</button>
          </th>
        </tr>
      </tbody>
    </table>

    <!-- create new todo -->
    <h2>Add new task</h2>
    <form class="add-form" v-on:submit.prevent="doAdd">
      Comment  <input type="text" ref="comment">
      <button type="submit">Add</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      todos: []
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
        id: this.$data.todos.length,
        comment: comment.value,
        isDone: false
      });
      // 入力値をリセット
      comment.value = '';
    },
    doChangeState(item) {
      item.isDone = item.isDone ? false : true;
    }
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
