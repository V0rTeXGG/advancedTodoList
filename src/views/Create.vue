<template>
<h1 class="title">Create task</h1>
  <form @submit.prevent="submitHandler" class="create-form">
    <div class="input-wrapper">
      <input
          v-model="title"
          @focus="this.error = false"
          id="title" type="text"
          class="input-wrapper__input">
      <label
          :class="{active: this.title.length > 0}"
          for="title"
          class="input-wrapper__label">
        Title</label>
      <p v-if="error" class="input-wrapper__error">Please fill in this field</p>
    </div>
    <div class="input-wrapper">
      <input
          v-model="tag"
          id="tags" type="text"
          class="input-wrapper__input">
      <label
          :class="{active: this.tag.length > 0}"
          for="tags"
          class="input-wrapper__label">
        tags</label>
      <div class="tags-wrapper">
        <span
            v-for="tag of tags"
            class="input-wrapper__tags">
          {{tag}}
          <button
              @click="remove(tag)"
              type="button"
              class="tags-wrapper__tags__delete">
          </button>
        </span>
      </div>
      <button
          @click.prevent.stop="addTag"
          type="submit"
          class="input-wrapper__add">Add</button>
    </div>
    <div class="input-wrapper">
      <textarea
          v-model="text"
          id="text" class="input-wrapper__input input-wrapper__text"
          maxlength="2048"></textarea>
      <label
          :class="{active: this.text.length > 0}"
          for="text"
          class="input-wrapper__label">
        Text</label>
      <label
          :class="{error: counterWord === 2048}"
          for="text"
          class="text-counter">
        {{counterWord}} / 2048</label>
    </div>
    <div class="input-wrapper">
      <input
          v-model="date"
          type="date"
          id="Test_DatetimeLocal"
          class="input-wrapper__input input-wrapper__date">
    </div>
      <button type="submit" class="create-form__btn"><span class="create-form__btn__text">Create task</span></button>
  </form>
    <span v-if="addTask" class="create-massage">Add task</span>
</template>

<script>
export default {

  name: 'create',

  data() {
    return {
      show: true,
      title: '',
      tag: '',
      text: '',
      date: '',
      tags: [],
      error: false,
      addTask: false,
    }
  },
  methods: {
    submitHandler() {
      if(this.title === '') {
        return this.error = true;
      } else {
        const task = {
          title: this.title,
          tags: this.tags,
          description: this.text,
          id: Date.now(),
          status: 'active',
          date: this.date,
        }
        this.title = '';
        this.tag = '';
        this.text = '';
        this.tags = [];
        this.date = '';

        let timer = setTimeout(() => this.addTask = true, 0);
       setTimeout(() => {
         clearTimeout(timer);
         this.addTask = false;
       }, 3000)
        this.$store.dispatch('createTask', task);
      }
    },
    addTag() {
      if(this.tag === '') {
        return;
      }
      this.tags.push(this.tag);
      this.tag = '';
    },
    remove(tag) {
      this.tags = this.tags.filter(item => item !== tag);
    }
  },
  computed: {
    counterWord() {
      return this.text.length;
    }
  },
  watch: {
    text() {
      if(this.text.length >= 2048) {
      this.text = this.text.slice(0, 2048);
      }
    },
  }
}
</script>

<style src="../style/create.scss" lang="scss" scoped>

</style>