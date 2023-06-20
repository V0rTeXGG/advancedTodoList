<template>
  <template v-if="task">
    <h1 class="title"> Task: {{ task.title }}</h1>
    <form @submit.prevent="submitHandler" class="task-form">
      <div class="tags-wrapper">
        <span class="tags-wrapper__title">Tags:</span>
        <span
            v-for="tag in tags"
            class="input-wrapper__tags">
          {{tag}}
        </span>
      </div>
      <div class="input-wrapper">
      <textarea
          v-model="text"
          :readonly="this.task.status == 'completed'"
          unselectable="on"
          id="text" class="input-wrapper__input input-wrapper__text"
          maxlength="2048"></textarea>
      <label
          :class="{active: this.text.length > 0}"
          for="text"
          class="input-wrapper__label">
        Text</label>
      <label
          :class="{error: this.text.length === 2048}"
          for="text"
          class="text-counter">
        {{this.text.length}} / 2048</label>
    </div>
    <div class="input-wrapper">
      <input
          v-model="date"
          :readonly="this.task.status == 'completed'"
          type="date"
          id="Test_DatetimeLocal"
          class="input-wrapper__input input-wrapper__date">
    </div>
    <span class="input-wrapper__status">{{task.status}}
      <button v-if="this.task.status == 'active'" @click.prevent="completeTask"  type="button" class="input-wrapper__btn-status">
        <span class="">Complete task</span>
      </button>
    </span>
    <button v-if="this.task.status == 'active'" type="submit" class="create-form__btn">
      <span class="create-form__btn__text">Change task</span>
    </button>
    </form>
  </template>
  <span v-else>Task not found</span>
</template>

<script>
export default {
  name: 'Task',
  data() {
    return {
      text: '',
      tags: [],
      date: '',
    }
  },
  computed: {
    tasks() {
      return this.$store.getters.tasks
    },
    task() {
      return this.$store.getters.taskById(+this.$route.params.id)
    },

  },
  mounted() {
      this.tags = this.task.tags;
      this.text = this.task.description;
      this.date = this.task.date;
  },
  methods: {
    submitHandler() {
      this.$store.dispatch('updateTask', {
      id: this.task.id,
      description: this.text,
      date: this.date,
    })
    },
    completeTask() {
      this.$store.dispatch('completeTask', this.task.id)

    }
  }
}
</script>

<style src="@/style/task.scss" lang="scss" scoped>

</style>