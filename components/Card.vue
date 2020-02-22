<template>
  <div class="column">
    <div class="card container">
      <vue-element-loading :active="isLoading" spinner="spinner" color="#FF6700"/>
      <header class="card-header">
        <p class="card-header-title has-text-grey">
          {{ title }}
        </p>
        <p class="card-header-icon" @click="refreshLetterStatus()">
          <b-icon size="is-small" icon="refresh"/>
        </p>
        <p class="card-header-icon" @click="deleteLetter()">
          <b-icon size="is-small" icon="delete"/>
        </p>
      </header>
      <div class="card-content">
        <div class="content has-text-centered">
          tracking: {{ this.trackingNumber }}
        </div>
        <div class="content has-text-centered">
          status: {{ this.status }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VueElementLoading from 'vue-element-loading'

export default {
  components: {
    VueElementLoading
  },
  props: {
    title: {
      type: String,
      required: true
    },
    id: {
      type: Number,
      required: true
    },
    trackingNumber: {
      type: String,
      required: true
    },
    status: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      iconByType: {

      },
      isLoading: false
    }
  },
  methods: {
    deleteLetter () {
      this.$buefy.dialog.confirm({
        title: 'Deleting letter',
        message: `Are you sure you want to delete this letter (id: ${this.id}) ? This action cannot be undone.`,
        confirmText: `Delete letter with id ${this.id} field`,
        type: 'is-danger',
        hasIcon: true,
        scroll: 'keep',
        onConfirm: async () => {
          await this.$axios.delete(`letters/${this.id}`)
          this.$buefy.toast.open({
            message: `Letter with <b>id: ${this.id}</b> deleted`,
            type: 'is-success'
          })
          this.$emit('delete')
        }
      })
    },
    async refreshLetterStatus() {
      this.isLoading = true
      await this.$axios.patch(`letters/${this.id}/status`).then(async (response) => {
      }).catch((error) => {
        this.$buefy.toast.open({
          duration: 5000,
          message: `Something went wrong: ${error.message}`,
          type: 'is-danger'
        })
      })
      this.$emit('update')
      this.isLoading = false
    }
  }
}
</script>

<style>
.content {
  font-size: 0.8rem;
}
</style>