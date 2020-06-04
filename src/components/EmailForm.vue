<template>
  <form @submit.prevent="generateLink" class="form">
    <label>
      <span>To:</span>
      <input type="email" v-model="recipient"/>
    </label>
    <label>
      <span>Subject:</span>
      <input type="text" v-model="subject"/>
    </label>
    <label>
      <span>Body:</span>
      <textarea v-model="body"/>
    </label>
    <button type="submit">Create Auto Email Link</button>
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
      recipient: 'collinclarke@gmail.com',
      subject: 'hello',
      body: 'humongous rediculous body'
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
    font-size: 2rem;
    display: flex;
    flex-flow: column nowrap;
    max-width: 768px;
    margin: 0 auto;
    label {
      display: flex;
      flex-flow: column nowrap;
      text-align: left;
      width: 100%;
      margin-bottom: 1rem;
    }
    input, textarea {
      font-size: inherit;
      width: 100%;
    }
  }
</style>