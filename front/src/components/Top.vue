<template>
  <div>
    <p>{{ message }}</p>
    <p>{{ message2 }}</p>
    <button v-on:click="changeMsg">Change</button>
    <ul>
      <li v-for="data in list" :key="data.id">{{ data.title }}</li>
    </ul>
    <button v-on:click="listSnippet">listSnippet</button>
    <button v-on:click="postSnippet">postSnippet</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      message: 'Hello Vue!',
      message2: 'Hoge',
      list: [{title:'ruby'}, {title:'title2'}]
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
      axios.post('http://localhost:3000/snippets', {title: 'new title', language: 'Ruby', contents: 'contents'})
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
</style>
