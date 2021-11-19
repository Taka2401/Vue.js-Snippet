<template>
  <div>
    <p>{{ message }}</p>
    <p>{{ message2 }}</p>
    <button v-on:click="changeMsg">Change</button>
  </div>
</template>

<script>
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
      // 連想配列での代入
      this.message2 = this.list[0]['title']
    }
  }
}
</script>

<style>
  button {
    margin-top: 50px;
  }
</style>
