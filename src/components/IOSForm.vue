<template>
  <form @submit.prevent="generateLink" class="form">
    <label>
      <span>To:</span>
      <input placeholder="55156" type="tel" v-model="recipient"/>
    </label>
    <label>
      <span>Body:</span>
      <textarea rows="5" v-model="body"/>
    </label>
    <button type="submit">Generate Auto Text Link</button>
  </form>
</template>

<script>
import urlencode from 'urlencode'
export default {
  data () {
    return {
      recipient: '',
      body: 'Replace text here with your text message.\n\n'
    }
  },
  methods: {
    generateLink() {
      const body = urlencode(this.body)
      const link = `sms:${this.recipient}&body=${body}`
      this.$emit('linkGenerated', {
        link
      })
    }
  }

}
</script>

<style lang="scss">
  .form {
    display: flex;
    flex-flow: column nowrap;
  }
</style>