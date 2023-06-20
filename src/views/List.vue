<template>
<h1 class="title">List tasks</h1>
  <aside class="wrapper-filters">
    <h3 class="wrapper-filters-title">Filters</h3>
  <select v-model="filter" @change="handleFilter" class="wrapper-filters__filter">
    <option value="starter" selected disabled class="wrapper-filters__filter__option">Starter </option>
    <option value="active" class="wrapper-filters__filter__option">Active</option>
    <option value="completed" class="wrapper-filters__filter__option">Completed</option>
    <option value="outdated" class="wrapper-filters__filter__option">Outdated</option>
  </select>
  <button @click="this.filter = ''; handleFilter()" class="wrapper-filters__reset">Reset filter</button>
  <div @keyup.enter="addTag" class="input-wrapper">
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
            v-for="tag in tags"
            class="input-wrapper__tags">
          {{tag}}
          <button
              @click.prevent="remove(tag)"
              type="button"
              class="tags-wrapper__tags__delete">
          </button>
        </span>
      </div>
      <button
          @click.prevent.stop="addTag"
          type="button"
          class="input-wrapper__add">Add</button>
    </div>
  </aside>
  <div v-if="tasks.length > 0" class="list-table">
    <div class="list-table__titles">
      <span class="list-table__titles__title">id</span>
      <span class="list-table__titles__title">title</span>
      <span class="list-table__titles__title">description</span>
      <span class="list-table__titles__title">date</span>
      <span class="list-table__titles__title">status</span>
      <span class="list-table__titles__title">Open</span>
    </div>
    <div v-for="(task, id) in filterTasks" :key="task.id" class="list-table__item">
      <span class="list-table__item__title">{{id+1}}</span>
      <span class="list-table__item__title">{{task.title}}</span>
      <span class="list-table__item__title description">{{task.description}}</span>
      <span class="list-table__item__title">{{new Date(task.date).toLocaleDateString()}}</span>
      <span class="list-table__item__title">{{task.status}}</span>
      <router-link :to="'/task/' + task.id" tag="button" class="list-table__button">Open</router-link>
    </div>
  </div>
  <p v-else="" class="list-message-empty">There are no tasks</p>
</template>

<script>
export default {
  data() {
    return {
      filter: '',
      tag: '',
      tags: [],
    }
  },
  computed: {
    tasks() {
      return this.$store.getters.tasks
    },
    filterTasks() {
      return this.tasks.filter(task => {
        if(this.tags.length === 0) {
          return true
        }
        return task.tags.some(tag => this.tags.includes(tag));
      }).filter(task => {
        if(this.filter === '') {
          return true
        }
          return task.status === this.filter
      });
    },
    addTag() {
      if(this.tag === '') {
        return;
      }
      this.tags.push(this.tag.toLowerCase());
      this.tag = '';
      localStorage.setItem('tags', JSON.stringify(this.tags));
    },
  },
  methods: {
    remove(tag) {
      this.tags = this.tags.filter(item => item !== tag);
      localStorage.setItem('tags', JSON.stringify(this.tags));
    },
    handleFilter() {
      let selectedValue = this.filter;
      localStorage.setItem('selectedTag', selectedValue);
    },
  },
  mounted() {
    const selectedValue = localStorage.getItem('selectedTag');
    const savedTags = localStorage.getItem('tags');
    if (selectedValue) {
      this.filter = selectedValue;
    }
    if (savedTags) {
      this.tags = JSON.parse(savedTags);
    }
  }
}
</script>

<style src="@/style/list.scss" lang="scss" scoped>

</style>