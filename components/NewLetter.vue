<template>
  <div class="modal-card">
    <header class="modal-card-head">
        <p class="modal-card-title">Track a new letter</p>
    </header>
    <section class="modal-card-body">
      <b-input type="text" v-model="trackingNumber" placeholder="3C00478618820" />
    </section>
    <footer class="modal-card-foot">
        <button class="button" type="button" @click="$parent.close()">Close</button>
        <button class="button is-info" @click="createNewLetter">Create</button>
    </footer>
  </div>
</template>

<script>
export default {
  data () {
    return {
      trackingNumber: '',
    }
  },
  methods: {
    createNewLetter() {
      this.$axios.post('letters', {
        'tracking_number': this.trackingNumber
      }).then((response) => {
        this.$emit('created')
        this.$parent.close()
        this.$buefy.toast.open({
          duration: 5000,
          message: 'Letter tracked !',
          type: 'is-success'
        })
      }).catch((error) => {
        this.$buefy.toast.open({
          duration: 5000,
          message: `Something went wrong: ${error.message}`,
          type: 'is-danger'
        })
      })
    }
  }
}
  
</script>

<style>
  
</style>