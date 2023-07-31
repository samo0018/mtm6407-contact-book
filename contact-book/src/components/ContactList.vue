<script setup>
import { ref, reactive } from 'vue'

const contacts = reactive([
  { id: 1, firstname: 'Adam', lastname: 'Smith', phone: '250-778-2222', email: 'adam.smith80@hotmail.com' },
  { id: 2, firstname: 'Mary', lastname: 'Jones', phone: '250-226-4562', email: 'mary.jones@gmail.com' },
  { id: 3, firstname: 'Lucy', lastname: 'Brown', phone: '250-889-6875', email: 'lucy_b@hotmail.com' },
  { id: 4, firstname: 'Margie', lastname: 'Adams', phone: '250-543-2658', email: 'margiee46@hotmail.com' },
  { id: 5, firstname: 'George', lastname: 'Lark', phone: '250-545-2356', email: 'g_lark@gmail.com' },
]);

contacts.sort((a, b) => a.lastname.localeCompare(b.lastname))

function removeContact(contactId) {
  const index = contacts.findIndex((contact) => contact.id === contactId)
  if (index !== -1) {
      setTimeout(() => {
      contacts.splice(index, 1)
    }, 200)
  }
}

function editContact(contactId) {

}

function toggleDetails(contactId) {
      const contact = contacts.find((contact) => contact.id === contactId)
      if (contact) {
            contact.showDetails = !contact.showDetails
      }
}

</script>

<template>
  <div class="contacts-container">
    <div class="top-bar">
      <input v-model="searchQuery" type="text" class="searchbar" placeholder="Search" />
      <button class="add-new-button">
        <span class="add-icon"></span>
        Add Contact
      </button>
    </div>

    <ul class="contacts-list">
      <li v-for="contact in contacts" :key="contact.id">
            <div class="each-contact">
                  <div class="profile-icon-name" @click="toggleDetails(contact.id)">
                        <img alt="contact book icon" class="profile-icon" src="@/assets/default-profile.svg"/>
                        <h3>{{ contact.firstname }} {{ contact.lastname }}</h3>
                  </div>
                  <div class="buttons-container">
                        <button class="edit-contact-button"  @click="editContact(contact.id)">
                              <span class="edit-icon"></span>
                        </button>
                        <button class="delete-contact-button" @click="removeContact(contact.id)">
                              <span class="delete-icon"></span>
                        </button>
                  </div>
            </div>
            <div v-if="contact.showDetails">
              <p>Phone: {{ contact.phone }}</p>
              <p>Email: {{ contact.email }}</p>
            </div>
      </li>
    </ul>
  </div>
</template>

<style scoped>

h3 {
      font-weight: 500;
      font-size: 1.5rem;
}
.contacts-container {
      background-color: rgb(212, 212, 212);
      margin-top: 20px;
}
.top-bar {
      display: flex;
      padding: 20px;
}
.contacts-list {
    list-style-type: none;
    background-color: rgb(255, 255, 255);
    padding-top: 20px;
}
.each-contact {
      display: flex;
      justify-content: space-between;
}

li {
      font-size: 1.3rem;
      background-color: rgb(243, 243, 243);
      margin-right: 40px;
      margin-bottom: 20px;
      padding: 20px 20px 20px 30px;
      border-radius: 6px;
      cursor: pointer;
}

li:nth-child(even) {
    background-color: rgb(231, 231, 231);
}
.add-new-button {
  display: inline-flex;
  align-items: center;
  padding: 10px;
  padding-right: 12px;
  margin-left: 20px;
  color: rgb(255, 255, 255);
  background-color: rgb(52, 145, 66);
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
}
.add-new-button:active {
      background-color: rgb(14, 67, 22);
}
.add-icon {
  width: 20px;
  height: 20px;
  margin-right: 8px;
  background-image: url('src/assets/add-button.svg');
  background-size: cover;
  background-repeat: no-repeat;
}

.searchbar {
      width: 50%;
      border-radius: 20px;
      border: none;
      padding-left: 20px;
      font-size: 18px;
}

input::placeholder {
    font-size: 17px;
    color: rgb(120, 120, 120);
    font-style: italic;
}
input:focus {
  outline: none;
}
.delete-contact-button {
  display: inline-flex;
  align-items: center;
  padding: 10px;
  margin-left: 20px;
  background-color: rgb(142, 53, 53);
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.delete-contact-button:active {
      background-color: rgb(95, 34, 34);
}
.delete-icon {
  width: 20px;
  height: 20px;
  background-image: url('src/assets/minus-button.svg');
  background-size: cover;
  background-repeat: no-repeat;
}
.edit-contact-button {
  display: inline-flex;
  align-items: center;
  padding: 10px;
  background-color: rgb(55, 55, 55);
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.edit-contact-button:active {
      background-color: rgb(33, 33, 33);
}
.edit-icon {
  width: 20px;
  height: 20px;
  background-image: url('src/assets/edit-button.svg');
  background-size: cover;
  background-repeat: no-repeat;
}
.profile-icon-name {
      display: inline-flex;
      align-items: center;
}
.profile-icon {
  width: 40px;
  height: 40px;
  margin-right: 18px;
  background-image: url('src/assets/default-profile.svg');
  background-size: cover;
  background-repeat: no-repeat;
}
</style>
