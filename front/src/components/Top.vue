<template>
  <div>
    <p>{{ message }}</p>
    <p>{{ message2 }}</p>
    <button v-on:click="changeMsg">Change</button>
    <ul>
      <li v-for="data in list" :key="data.id">{{ data.title }}</li>
    </ul>
    <button v-on:click="listSnippet">listSnippet</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      message: 'Hello Vue!',
      message2: 'Hoge',
      list: [{title:'title1'}, {title:'title2'}]
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
      axios.get('/snippets.json')
      .then(response => (
          this.message2 = response.data[0]['title']
        )
      )
    },
    listSnippet() {
      axios.get('/snippets.json')
        .then(response => (
          this.list = response.data
        )
      )
    }
  }
}
</script>

<style>
  button {
    margin-top: 50px;
  }
  li {
    margin: 0 auto;
  }
</style>
