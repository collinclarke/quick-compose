<template>
  <form @submit.prevent="generateLink" class="form">
    <label>
      <span>To:</span>
      <input placeholder="email@example.com" type="text" v-model="recipient"/>
    </label>
    <label>
      <span>Subject:</span>
      <input placeholder="Subject of your email" type="text" v-model="subject"/>
    </label>
    <label>
      <span>Body:</span>
      <textarea rows="5" v-model="body"/>
    </label>
    <button type="submit">Generate Auto Email Link</button>
  </form>
</template>

<script>
import urlencode from 'urlencode'
export default {
  data () {
    return {
      recipient: '',
      subject: '',
      body: 'Replace text here with your email template.\n\n'
    }
  },
  methods: {
    generateLink() {
      console.log(this.recipient, this.subject, this.body)
      const body = urlencode(this.body)
      const subject = urlencode(this.subject)
      const link = `mailto:${this.recipient}?subject=${subject}&body=${body}`
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