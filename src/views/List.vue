<template>
  <div>
    <h1>List</h1>
    <div class="row">
      <div class="input-field col s6">
        <select ref="select" v-model="filter">
          <option value="" selected>Choose your option</option>
          <option value="active">Active</option>
          <option value="outdated">Outdated</option>
          <option value="completed">Completed</option>
        </select>
        <label>Status filter</label>
      </div>
    </div>
    <hr>
    <table v-if="tasks.length">
      <thead>
        <tr>
          <th>#</th>
          <th>Title</th>
          <th>Date</th>
          <th>Description</th>
          <th>Status</th>
          <th>Open</th>
        </tr>
      </thead>
      <tbody>
        <tr 
          v-for="(task, index) of displayTasks"
          :key="task.id"
        >
          <td>{{index + 1}}</td>
          <td class="td">
            <div class="text" :title="task.title">{{task.title}}</div>
          </td>
          <td>{{new Date(task.date).toLocaleDateString()}}</td>
          <td class="td">
            <div class="text" :title="task.description">{{task.description}}</div>
          </td>
          <td>{{task.status}}</td>
          <td>
            <router-link tag="button" class="btn btn-small blue darken-1" :to="'/task/' + task.id">
              Open
            </router-link>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>No tasks</p>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'

  export default {
    data: () => ({
      filter: null
    }),
    computed: {
      ...mapGetters(['tasks']),
      displayTasks() {
        return this.tasks.filter(t => {
          if(!this.filter) 
            return true
          return t.status === this.filter
        })
      }
    },
    mounted() {
      M.FormSelect.init(this.$refs.select, {})
    }
  }
</script>

<style lang="scss" scoped>
  .text {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  .td {
    max-width: 200px;
  }
</style>
