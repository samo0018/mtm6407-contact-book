<script setup>
defineProps({
  msg: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="contact-header-text">
    <h1 class="black">{{ msg }}</h1>
    <h3 class="italics">Click on a contact to see more info.</h3>
  </div>
</template>

<style scoped>
.contact-header-text {
  margin-top: 15px;
  margin-left: 10px;
}
</style>
