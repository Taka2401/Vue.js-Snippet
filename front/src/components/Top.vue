<template>
  <div>
   <div>
     <div v-if="status">setMessage</div>
     <div v-else>changedMessage</div>
   </div>
   <button @click="ChangeStatus">change</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      status: true
    }
  },
  methods: {
    ChangeStatus() {
      this.status = !this.status
    }
  }
}
</script>

<style>
  button {
    margin-top: 50px;
  }
</style>
