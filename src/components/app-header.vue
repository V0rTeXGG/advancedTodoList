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
    </div>
  </header>
</template>

<script>
export default {
  name: 'app-header',
}
</script>

<style src="@/style/header.scss" lang="scss" scoped>

</style>