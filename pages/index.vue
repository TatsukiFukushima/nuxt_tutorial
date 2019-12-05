<template>
  <section class="container">
    <h1>Todo App</h1>
    <p><input type="text" name="content" v-model="content" /></p>
    <div>
      <button @click="insert">save</button>
      <button @click="find">find</button>
    </div>
    <ul>
      <li v-for="(todo, index) in display_todos" :key="index">
        <span class="todo__content">{{ todo.content }}</span><br>
        <span class="todo__created">{{ todo.created }}</span><span class="todo__remove" @click="remove(todo)">delete</span>
      </li>
    </ul>
  </section>
</template>

<script>
import {mapState} from 'vuex';

export default {
  data: function() {
    return {
      content: '',
      search_word: '',
      find_flg: false
    }
  },
  computed: {
    ...mapState(['todos']),
    display_todos: function() {
      if(this.find_flg) {
        var arr = [];
        var data = this.todos;
        data.forEach(element => {
          if(element.content.toLowerCase() == this.search_word.toLowerCase()) {
            arr.push(element);
          }
        });
        return arr;
      } else {
        return this.todos;
      }
    }
  },
  methods: {
    insert: function() {
      this.$store.commit('insert', {content: this.content});
      this.content = '';
      this.find_flg = false;
    },
    find: function() {
      this.search_word = this.content
      this.find_flg = this.content != '';
    },
    remove: function(todo) {
      this.$store.commit('remove', todo)
    }
  }
}
</script>

<style>
</style>
