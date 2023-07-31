<script setup>
import ContactHeader from './components/ContactHeader.vue'
import ContactList from './components/ContactList.vue'
</script>

<template>
  <div>
    <header>
    <div class="contact-header">
      <img alt="contact book icon" class="contact-icon" src="@/assets/contact-book.svg" width="100" height="100"/>
      <div class="wrapper">
        <ContactHeader msg="Contact Book" />
      </div>
    </div>
  </header>

  <body>
    <ContactList />
  </body>
  </div>
</template>


<style scoped>
.contact-header {
  line-height: 1.5;
  max-height: 100vh;
  max-width: 100%;
  display: flex;
}
.contact-icon {
  display: block;
}
</style>
