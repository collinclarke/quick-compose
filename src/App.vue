<template>
  <main id="app">
    <section>
      <h2>Use the form below to generate an auto compose email.</h2>
      <EmailForm @linkGenerated="updateLink" />
    </section>
    <section :class="{ hidden: !link.trim(), animate: true }">
      <p :class="{ hidden: !showNotification, animate: true  }">{{ notification }}</p>
      <input ref="link" :readonly="true" :value="link" class="copy-link" />
      <button type="button" @click="copyLink" class="copy-link-button">Copy Link</button>
    </section>
    <section>
      <h2>Paste the above link into one of these link shortner services to generate a usable link</h2>
      <ul class="link-list">
        <li class="link-item">
          <a href="https://tinyurl.com/" target="_blank">Tiny Url</a>
        </li>
        <li>
          <a href="http://bit.ly/" target="_blank">Bitly</a>
        </li>
      </ul>
    </section>
  </main>
</template>

<script>
import EmailForm from './components/EmailForm.vue'

export default {
  name: 'App',
  components: {
    EmailForm
  },
  data () {
    return {
      link: '',
      notification: '',
      showNotification: false
    }
  },
  methods: {
    async updateLink({ link, api }) {
      switch (api) {
        default:
          if (this.link) {
            this.setNotification('Link Updated')
          }
          this.link = link;
          break;
      }
    },
    copyLink() {
      this.$refs.link.select()
      document.execCommand("copy")
    },
    setNotification (text) {
      this.notification = text
      this.showNotification = true
      setTimeout(() => {
        this.showNotification = false
      }, 3000)
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 1rem;
}
* {
  font-family: inherit;
  text-align: left;
}
.animate {
  transition: all 200ms;
}
.hidden {
  opacity: 0;
}
section {
  margin-bottom: 2rem;
}
.link-list {
  list-style: none;
  margin: 0; padding: 0;
}
.copy-link {
  padding: 2rem;
  width: 100%;
  margin-bottom: 1rem;
}
</style>
