<template>
  <section class="section">
    <div class="row has-text-centered">
      <b-button size="is-info" icon-right="refresh" @click="refreshLettersStatus">Refresh all</b-button>
      <vue-element-loading :active="isLoading" :is-full-screen="true" color="#FF6700" spinner="spinner"/>
      <b-button size="is-info" icon-right="plus" @click="isComponentModalActive = true">Track new letter</b-button>
      <b-modal :active.sync="isComponentModalActive" has-modal-card trap-focus aria-role="dialog" aria-modal>
        <new-letter @created="updateLetters" />
      </b-modal>
    </div>
    <div class="columns is-mobile is-multiline">
      <div class="column is-one-third" v-for="(letter, index) in this.letters">
        <card :title="`${letter.id}`" :trackingNumber="letter['tracking_number']" :id="letter.id" :status="letter.status ? letter.status : 'undefined'" @delete="updateLetters()" @update="updateLetter(letter.id, index)"/>
      </div>
    </div>
  </section>
</template>

<script>
import Card from '~/components/Card'
import NewLetter from '~/components/NewLetter'
import VueElementLoading from 'vue-element-loading'

export default {
  components: {
    Card,
    NewLetter,
    VueElementLoading
  },
  data () {
    return {
      letters: [],
      isLoading: false,
      isComponentModalActive: false
    }
  },
  mounted () {
    this.updateLetters()
  },
  methods: {
    async updateLetters() {
      await this.$axios.get('letters').then((response) => {
        this.letters = response.data
      });
    },
    updateLetter(id, lettersIndex) {
      this.$axios.get(`letters/${id}`).then((response) => {
        this.$set(this.letters, lettersIndex, response.data)
      });
    },
    refreshLettersStatus() {
      this.isLoading = true
      this.$axios.patch(`letters/status`).then(async (response) => {
        this.updateLetters().then(() => {
          this.$buefy.toast.open({
            duration: 5000,
            message: 'Letters updated !',
            type: 'is-success'
          })
        })
      }).catch((error) => {
        this.$buefy.toast.open({
          duration: 5000,
          message: `Something went wrong: ${error.message}`,
          type: 'is-danger'
        })
      }).finally(() => {
        this.isLoading = false
      })
    }
  },
}
</script>
