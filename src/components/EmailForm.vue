<template>
  <form @submit.prevent="generateLink" class="form">
    <label>
      <span>To:</span>
      <input placeholder="email@example.com" type="email" v-model="recipient"/>
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
const apiOptions = [
  {
    label: 'Raw Url',
    value: 'raw'
  },
  {
    label: 'Bitly',
    value: 'bitly'
  }
] 
export default {
  data () {
    return {
      apiOptions,
      optionIndex: 0,
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
        link,
        api: apiOptions[this.optionIndex].value
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