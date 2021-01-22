<template>
  <div class="row">
    <div v-if="task" class="col s6 offset-s3">
      <h1 class="text" :title="task.title">{{task.title}}</h1>
      <form @submit.prevent="submitHandler">
        <div class="chips" ref="chips"></div>
        <div class="input-field">
          <textarea v-model="description" id="description" class="materialize-textarea"></textarea>
          <label for="description">Description</label>
          <span class="character-counter" style="float: right; font-size: 12px;">{{ description.length }}/2048</span>
        </div>
        <input type="text" ref="datepicker">
        <div v-if="task.status !== 'completed'">
          <button class="btn btn-update" type="submit">Update</button>
          <button class="btn blue darken-1" type="button" @click="completeTask">Complete task</button>
        </div>
      </form>
    </div>
    <p v-else>Task not found</p>
  </div>
</template>

<script>
  import { mapActions } from 'vuex'

  export default {
    data: () => ({
      description: '',
      chips: null,
      date: null
    }),
    mounted() {
      this.description = this.task.description
      this.chips = M.Chips.init(this.$refs.chips, { 
        placeholder: 'Task tags',
        data: this.task.tags
      })
      this.date = M.Datepicker.init(this.$refs.datepicker, {
        format: 'dd.mm.yyyy',
        defaultDate: new Date(this.task.date),
        setDefaultDate: true
      })
      setTimeout(() => {
        M.updateTextFields()
      }, 0)
    },
    methods: {
      ...mapActions(['updateTask']),
      submitHandler() {
        this.updateTask({
          id: this.task.id,
          description: this.description,
          date: this.date.date
        })
        this.$router.push('/list')
      },
      completeTask() {
        this.$store.dispatch('completeTask', this.task.id)
        this.$router.push('/list')
      }
    },
    destroyed() {
      if(this.date && this.date.destroy)
        this.date.destroy()
      if(this.chips && this.chips.destroy)
        this.chips.destroy()
    },
    computed: {
      task() {
        return this.$store.getters.taskById(+this.$route.params.id)
      }
    }
  }
</script>

<style lang="scss" scoped>
  .text {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  .btn-update {
    margin-right: 1rem;
  }
</style>
