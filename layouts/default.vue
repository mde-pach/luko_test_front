<template>
  <div>
    <b-navbar>
      <template slot="brand">
          <b-navbar-item tag="router-link" :to="{ path: '/' }">
            <img
              src="~assets/luko_ft_laposte.png"
              alt="Buefy"
              height="28"
            >
          </b-navbar-item>
      </template>
      <template slot="start">
          <b-navbar-item target="_blank" href="https://developer.laposte.fr/products/suivi/2">
            La Poste API documentation
          </b-navbar-item>
          <b-navbar-item target="_blank" href="https://github.com/mde-pach/luko_test">
            Backend github repository
          </b-navbar-item>
          <b-navbar-item target="_blank" href="https://github.com/mde-pach/luko_test_front">
            Frontend github repository
          </b-navbar-item>
      </template>
    </b-navbar>
    <div class="container column is-10">
      <nuxt />
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
    }
  }
}
</script>
