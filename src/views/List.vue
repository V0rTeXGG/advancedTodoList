<template>
<h1 class="title">List tasks</h1>
  <aside class="wrapper-filters">
    <h3 class="wrapper-filters-title">Filters</h3>
  <select v-model="filter" @change="handleFilterStatus" class="wrapper-filters__filter">
    <option value="starter" selected disabled class="wrapper-filters__filter__option">Starter </option>
    <option value="active" class="wrapper-filters__filter__option">Active</option>
    <option value="completed" class="wrapper-filters__filter__option">Completed</option>
    <option value="outdated" class="wrapper-filters__filter__option">Outdated</option>
  </select>
  <button @click="this.filter = ''; handleFilterStatus()" class="wrapper-filters__reset">Reset filter status</button>
  <div class="input-wrapper">
    <input
    v-model="date"
    @change="handlerFilterDate"
    type="date"
    class="wrapper-filters__date input-wrapper__input">
  </div>
  <button @click="this.date = '';  handlerFilterDate()" class="wrapper-filters__reset">Reset filter date</button>
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
    <div v-for="(task, id) in paginatedTasks" :key="task.id" class="list-table__item">
      <span class="list-table__item__title">{{id+1}}</span>
      <span class="list-table__item__title">{{task.title}}</span>
      <span class="list-table__item__title description">{{task.description}}</span>
      <span class="list-table__item__title">{{new Date(task.date).toLocaleDateString()}}</span>
      <span class="list-table__item__title">{{task.status}}</span>
      <router-link :to="'/task/' + task.id" tag="button" class="list-table__button">Open</router-link>
    </div>
    <div class="wrapper-pages">
      <button v-for="pageNumber in totalPage" :key="pageNumber" @click="currentPage = pageNumber; handlerPage()" class="wrapper-pages__page" :class="{active: pageNumber === Number(currentPage)}">{{pageNumber}}</button>
    </div>
    <h3 class="list-wrapper-title-daily">Daily task:</h3>
    <app-slider :breakpoints="{ 320:{slidesPerView: 1}, 450:{ slidesPerView:2 }, 650:{ slidesPerView:3, } }"></app-slider>
  </div>
  <p v-else="" class="list-message-empty">There are no tasks</p>
</template>

<script>
import AppSlider from "@/components/ui/app-slider.vue";

export default {
  components: {AppSlider},
  data() {
    return {
      filter: '',
      tag: '',
      date: '',
      tags: [],
      currentPage: 1,
      tasksPerPage: 10,
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
      }).filter(task => {
        if(this.date === '') {
          return true
        }
        return task.date === this.date
      });
    },
    totalPage() {
      return Math.ceil(this.filterTasks.length / 10)
    },
    paginatedTasks() {
      const startIndex = (this.currentPage - 1) * this.tasksPerPage;
      const endIndex = startIndex + this.tasksPerPage;
      return this.filterTasks.slice(startIndex, endIndex);
      localStorage.setItem('paginatedTasks', JSON.stringify(this.filterTasks));
    },
  },
  methods: {
    addTag() {
      if(this.tag === '') {
        return;
      }
      this.tags.push(this.tag.toLowerCase());
      this.tag = '';
      localStorage.setItem('tags', JSON.stringify(this.tags));
    },
    remove(tag) {
      this.tags = this.tags.filter(item => item !== tag);
      localStorage.setItem('tags', JSON.stringify(this.tags));
    },
    handleFilterStatus() {
      let selectedValue = this.filter;
      localStorage.setItem('selectedTag', selectedValue);
    },
    handlerFilterDate() {
      let selectedValueDate = this.date;
      localStorage.setItem('selectedDate', selectedValueDate)
    },
    handlerPage() {
      let currentPage = this.currentPage;
      localStorage.setItem('currentPage', currentPage)
    }
  },
  beforeMount() {
    let currentPage = localStorage.getItem('currentPage');
    this.currentPage = currentPage;
    let selectedValue = localStorage.getItem('selectedTag');
    let savedTags = localStorage.getItem('tags');
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