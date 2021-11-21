<template>
  <div>
    <p>{{ message }}</p>
    <p>{{ message2 }}</p>
    <button @click="changeMsg">Change</button>
    <ul>
      <li v-for="data in list" :key="data.id">{{ data.title }}</li>
    </ul>
    <button @click="listSnippet">listSnippet</button>
    <div class="post-form">
      <input v-model="title" placeholder="title">
      <input v-model="language" placeholder="language">
      <input v-model="contents" placeholder="contents">
    </div>
    <button @click="postSnippet">postSnippet</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      message: 'Hello Vue!',
      message2: 'Hoge',
      list: [{title:'ruby'}, {title:'title2'}],
      title: '',
      language: '',
      contents: ''
    }
  },
  mounted () {
    this.setMsg();
  },
  methods: {
    setMsg() {
      this.message = 'Set Message';
    },
    changeMsg() {
      this.message = 'Changed Message'
      axios.get('http://localhost:3000/snippets.json')
        .then(response => (
          this.message2 = response.data[0]['title']
        )
      )
    },
    listSnippet() {
      axios.get('http://localhost:3000/snippets.json')
        .then(response => (
          this.list = response.data
        )
      )
    },
    postSnippet() {
      axios.post('http://localhost:3000/snippets', {title: this.title, language: this.language, contents: this.contents})
        .then(
          this.listSnippet()
        )
    }
  }
}
</script>

<style>
  button {
    margin-top: 20px;
  }

  ul {
    list-style: none;
  }

  .post-form {
    margin: 20px;
  }
</style>
