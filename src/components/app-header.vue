<template>
  <header class="header">
    <div class="header-wrapper">
      <router-link to="/">
        <a href="#" class="header-logo">Home</a>
      </router-link>
      <nav class="header-nav">
        <ul class="header-nav__list">
          <router-link
              tag="li"
              to="/"
              exact
              active-class="active"
          >
            <a href="#" class="header-nav__list__link">Create</a>
          </router-link>
          <router-link
              tag="li"
              to="/list"
              active-class="active">
            <a href="#" class="header-nav__list__link">List</a>
          </router-link>
        </ul>
      </nav>
      <button @click="openProfile" :class="{active: isProfile}" class="header-btn">
        <span  class="header-btn__line"></span>
        <span  class="header-btn__line"></span>
        <span  class="header-btn__line"></span>
      </button>
    </div>
    <div :class="{active: isProfile}" class="header-profile">
        <div class="header-profile__block">
          <span class="header-profile__title">Total tasks:</span>
          <span class="header-profile__value">{{tasks.length}}</span>
        </div>
        <div class="header-profile__block">
          <span class="header-profile__title">Active task:</span>
          <span class="header-profile__value">{{activeTasks}} / {{tasks.length}}</span>
        </div>
        <div class="header-profile__block">
          <span class="header-profile__title">Completed task:</span>
          <span class="header-profile__value">{{completedTask}} / {{tasks.length}}</span>
        </div>
        <div class="header-profile__block">
          <span class="header-profile__title">Outdated task:</span>
          <span class="header-profile__value">{{outdatedTask}} / {{tasks.length}}</span>
        </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'app-header',

  data() {
    return {
      isProfile: false,
    }
  },
  methods: {
    openProfile() {
      this.isProfile = !this.isProfile;
      // document.documentElement.style.overflow = 'hidden';
    }
  },
  computed: {
    tasks() {
      return this.$store.getters.tasks
    },
    activeTasks() {
      return this.tasks.filter(task => task.status === 'active').length
    },
    completedTask() {
      return this.tasks.filter(task => task.status === 'completed').length
    },
    outdatedTask() {
      return this.tasks.filter(task => task.status === 'outdated').length
    }
  }
}
</script>

<style src="@/style/header.scss" lang="scss" scoped>

</style>