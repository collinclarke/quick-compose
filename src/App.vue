<template>
  <main id="app">
    <section>
      <h2>
        <span>Use the form below to generate an auto compose</span>
        <select class="inline-select" v-model="formtype">
          <option v-for="formOption in formtypes" :value="formOption" :key="formOption">{{ formOption }}</option>
        </select>
        <span>link.</span>
      </h2>
      <EmailForm v-show="formtype === 'email'" @linkGenerated="updateLink" />
      <IOSForm v-show="formtype === 'IOS text'" @linkGenerated="updateLink" />
      <AndroidForm v-show="formtype === 'Android text'" @linkGenerated="updateLink" />
    </section>
    <section :class="{ hidden: !link.trim(), animate: true }" class="copy-link-holder">
      <p :class="{ hidden: !showNotification, animate: true, notification: true }">{{ notification }}</p>
      <input ref="link" :readonly="true" :value="link" class="copy-link" />
      <button type="button" @click="copyLink" class="copy-link-button">Copy Link</button>
    </section>
    <section>
      <h2>Paste the above link into a url shortener like <a class="link" href="https://tinyurl.com/" target="_blank">Tiny Url</a> to generate a usable link for social media.</h2>
    </section>
  </main>
</template>

<script>
import EmailForm from './components/EmailForm.vue'
import IOSForm from './components/IOSForm.vue'
import AndroidForm from './components/AndroidForm.vue'

const formtypes = [ 'email', 'IOS text', 'Android text' ]

export default {
  name: 'App',
  components: {
    EmailForm,
    IOSForm,
    AndroidForm
  },
  data () {
    return {
      link: '',
      formtypes,
      formtype: formtypes[0],
      notification: '',
      showNotification: false
    }
  },
  methods: {
    async updateLink({ link, api }) {
      switch (api) {
        default:
          if (this.link) {
            this.setNotification('Link Updated!')
          } else {
            this.setNotification('Link Generated!')
          }
          this.link = link;
          break;
      }
    },
    copyLink() {
      this.$refs.link.select()
      document.execCommand("copy")
      this.setNotification('Link Copied!')
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
* {
  outline: 0;
  border: 0;
  box-shadow: none;
  font-family: inherit;
  font-size: inherit;
}
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  padding: 1rem;
  font-family: sans-serif;
}
.animate {
  transition: all 200ms;
}
.hidden {
  opacity: 0;
}
section {
  margin-bottom: 2rem;
  display: flex;
  flex-flow: column nowrap;
}
button {
  padding: 1rem;
  background: #f9f9f9;
  cursor: pointer;
  border: 1px solid #e1e1e1;
  &:hover, &:focus {
    color: #1515ff;
    background: #f4f4ff;
  }
}
label {
  display: flex;
  flex-flow: column nowrap;
  text-align: left;
  margin-bottom: 1rem;
  span {
    margin-bottom: .25rem;
  }
}
input, textarea {
  background: #f9f9f9;
  border: 1px solid #e1e1e1;
  padding: .5rem;
  resize: none;
  &:focus {
    border-color: #1515ff;
    background: #f4f4ff;
  }
}
.link-list {
  list-style: none;
  margin: 0; padding: 0;
  .link-item {
    padding: .25rem;
  }
}
.link {
  color: #1515ff;
  text-decoration: none;
  font-weight: bold;
  &:hover, &:focus {
    text-decoration: underline;
  }
}
.copy-link {
  padding: 1rem;
  margin-bottom: 1rem;
}
.copy-link-button{
  margin-left: auto;
}
.notification {
  padding: .25rem;
  color: #1515ff;
  text-align: center;
  width: auto;
}
.inline-select {
  margin: 0 .5rem;
}
</style>
