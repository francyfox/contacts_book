<template>
  <div class="wrap container is-max-desktop">
    <div class="column">
      <nav class="breadcrumb" aria-label="breadcrumbs">
        <nuxt-link to="/">
            <span class="icon is-small">
              <fa icon="home"/>
            </span>
          <span>Back to home</span>
        </nuxt-link>
      </nav>
      <div class="box content">
        <article class="post">
          <div class="media">
            <div class="media-content">
              <div class="content">
                <h1 class="title">
                  Name: {{ getInfo.name }}
                </h1>
                <h2 class="title is-5">
                  Phone: {{ getInfo.number }}
                </h2>
              </div>
            </div>
          </div>
        </article>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    getInfo () {
      const id = parseInt(this.$route.params.name)
      return JSON.parse(localStorage.contacts)[id]
    }
  }
}
</script>

<style scoped>

</style>
