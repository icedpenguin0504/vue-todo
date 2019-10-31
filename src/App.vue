<template lang="html">
  <div>
    <!-- show todos -->
    <table>
      <thead>
        <tr>
          <th class="id">ID</th>
          <th class="comment">Comment</th>
          <th class="state">State</th>
          <th class="button">-</th>
        </tr>
      </thead>
      <tbody>
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
      </tbody>
    </table>

    <!-- create new todo -->
    <h2>Add new task</h2>
    <form class="add-form" v-on:submit.prevent="doAdd">
      Comment<input type="text" ref="comment">
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
        id: this.$data.todos.length + 1,
        comment: comment.value,
        state: 0
      });
      // 入力値をリセット
      comment.value = '';
    }
  }
}
</script>

<style lang="css" scoped>
</style>
