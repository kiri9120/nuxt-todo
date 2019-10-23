<template>
   <v-container style="max-width: 500px;">
      <v-list>
         <v-subheader>Todoリスト</v-subheader>
         <v-list-item-group color="primary">
            <v-list-item v-for="todo in todos" :key="todo.id">
               <div v-if="todo.created" style="width: 100%;">
                  <v-row>
                     <v-col cols="2" md="1">
                        <v-checkbox v-bind:checked="todo.done" @change="toggle(todo)" :id="todo.id"></v-checkbox>
                     </v-col>
                     <v-col cols="8" class="d-flex align-center">
                        <label :for="todo.id" style="width: 100%;">
                           <span v-bind:class="{ done: todo.done }">
                           {{ todo.name }} <br>
                           {{ todo.created.toDate() | dateFilter }}
                           </span>
                        </label>
                     </v-col>
                     <v-col class="text-right d-flex align-center">
                        <v-btn color="primary" v-on:click="remove(todo.id)">削除</v-btn>
                     </v-col>
                  </v-row>
               </div>
            </v-list-item>
         </v-list-item-group>
      </v-list>
      <div class="form">
         <v-form v-on:submit.prevent="add">
            <v-text-field v-model="name" placeholder="新規タスク入力" style="width: 300px;"></v-text-field>
            <div class="text-right">
               <button>追加</button>
            </div>
         </v-form>
      </div>
   </v-container>
</template>

<script>
import moment from 'moment'
export default {
   data: function() {
      return {
         name: '',
         done: false
      }
   },
   created: function() {
      this.$store.dispatch('todos/init')
   },
   methods: {
      add() {
         this.$store.dispatch('todos/add', this.name)
         this.name = ''
      },
      remove(id) {
         this.$store.dispatch('todos/remove', id)
      },
      toggle(todo) {
         this.$store.dispatch('todos/toggle', todo)
      }
   },
   computed: {
      todos() {
         // return this.$store.state.todos.todos
         return this.$store.getters['todos/orderdTodos']
      }
   },
   filters: {
      dateFilter: function(date) {
         return moment(date).format('YYYY/MM/DD HH:mm:ss')
      }
   }
}
</script>

<style>
   span.done {
      text-decoration: line-through;
   }
   label {
      user-select: none;
   }
</style>