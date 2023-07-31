<script setup>
import { ref, reactive, computed } from 'vue'
import { onMounted } from 'vue'

// *************************************** CONTACTS ******************************************
const contacts = reactive([
  { id: 1, firstname: 'Adam', lastname: 'Smith', phone: '250-778-2222', email: 'adam.smith80@hotmail.com' },
  { id: 2, firstname: 'Mary', lastname: 'Jones', phone: '250-226-4562', email: 'mary.jones@gmail.com' },
  { id: 3, firstname: 'Lucy', lastname: 'Brown', phone: '250-889-6875', email: 'lucy_b@hotmail.com' },
  { id: 4, firstname: 'Margie', lastname: 'Adams', phone: '250-543-2658', email: 'margiee46@hotmail.com' },
  { id: 5, firstname: 'George', lastname: 'Lark', phone: '250-545-2356', email: 'g_lark@gmail.com' },
]);
// Sort my array of contacts alphabetically, by last name
contacts.sort((a, b) => a.lastname.localeCompare(b.lastname))

// *************************************** SEARCH A CONTACT ******************************************

// New const holds the value of the search input, default is blank
const searchQuery = ref('')

// We save this function as a new const because "computed" is already a function we can import from Vue (see line 2)
const filteredContacts = computed(() => {
  // Return a contact that has been filtered through the contacts array
  return contacts.filter((contact) => {
    const fullName = `${contact.firstname} ${contact.lastname}`.toLowerCase() // Using the filter method, we can create a "new" array that is simply the search result: first and last name combined into one (full name), and not case sensitive 
    return fullName.includes(searchQuery.value.toLowerCase()) // When the user types in the searchbar, it is also not case sensitive 
  })
})

// *************************************** ADD A CONTACT ******************************************

// Function to ADD a new contact when the user clicks the following button: "add-new-button"
function addNewContact() {
      const newContact = {
            id: contacts.length +1,
            firstname: '',
            lastname: '',
            phone: '',
            email: '',
            showDetails: false,
      }
      contacts.unshift(newContact) // I want the new contact added to the top rather than 'pushed' to the bottom of the list (better UX)
      setEditingContact(newContact) // Show the editable fields for the newly added contact
      saveContactsToLocalStorage() // Save to local storage (associated function below)
}

// SAVE the new contact to local storage
function saveContactsToLocalStorage() {
      localStorage.setItem('contacts', JSON.stringify(contacts))
}

// When the component is mounted, I want to make sure all the contacts load from local storage
// onMounted(() => {
//   const savedContacts = localStorage.getItem('contacts')
//   if (savedContacts) {
//     contacts.length = 0
//     contacts.push(...JSON.parse(savedContacts))
//   }
// })

// *************************************** EDIT A CONTACT ******************************************

// Before the user edits a contact, the default is for the editingContact value to be nothing
const editingContact = ref(null)

// Function to EDIT a contact when the user clicks the following button: "edit-contact-button" 
function setEditingContact(contact) {
  editingContact.value = { ...contact } // Copies the contact being edited
  toggleDetails(contact.id) // Shows the hidden details for the contact that is being edited (more intuitive)
}

// Function for the user to SAVE the edited contact
function saveEditedContact() {
  if (editingContact.value) { // Checks if the user is currently editing a contact from the array of contacts
    const index = contacts.findIndex((contact) => contact.id === editingContact.value.id)

    if (index !== -1) { // If the contact exists in the contacts array...
      contacts[index] = editingContact.value // Update it with the edited values
      saveContactsToLocalStorage() // Save it to localstorage 
      editingContact.value = null // Hide once the function completes
    }
  }
}

// Function for the user to CANCEL editing a contact
function cancelEdit() {
  // If there is currently a contact being edited, find the contact object in the array
  if (editingContact.value) {
    const contact = contacts.find((c) => c.id === editingContact.value.id)
    // If the contact is found in the array, then hide the contact details after cancelling the edit
    if (contact) {
      contact.showDetails = false
    }
    editingContact.value = null // Ends the process
  }
}

// *************************************** DELETE A CONTACT ******************************************

// Function to DELETE a contact when the user clicks the following button: "delete-contact-button"
function removeContact(contactId) {
  // Find the index of the contact we wish to remove from the contacts array
  const index = contacts.findIndex((contact) => contact.id === contactId)
  // If the contact exists in the array...
  if (index !== -1) {
      setTimeout(() => { 
      contacts.splice(index, 1) // Use the splice method to remove the selected contact from the contacts array
    }, 200) // Delay the removal by 200ms
  }
}

// *************************************** SHOW CONTACT DETAILS ******************************************

// Function to SHOW the contact's details when the contact's name is clicked: "profile-icon-name" (I wrapped the contact's icon SVG and name in a div so either can be clicked for this to work)
function toggleDetails(contactId) {
      const contact = contacts.find((contact) => contact.id === contactId)
      if (contact) {
            contact.showDetails = !contact.showDetails
      }
}
</script>

<!-- *************************************** MY VUE HTML TEMPLATE ****************************************** -->
<template>
  <div class="contacts-container">
    <div class="top-bar">
      <input v-model="searchQuery" type="text" class="searchbar" placeholder="Search" />
      <button class="add-new-button" @click="addNewContact">
        <span class="add-icon"></span>
        Add Contact
      </button>
    </div>

    <ul class="contacts-list">
      <li v-for="contact in filteredContacts" :key="contact.id">
            <div class="each-contact">
                  <div class="profile-icon-name" @click="toggleDetails(contact.id)">
                        <img alt="contact book icon" class="profile-icon" src="@/assets/default-profile.svg"/>
                        <h3>{{ contact.firstname }} {{ contact.lastname }}</h3>
                  </div>
                  
                  <div class="buttons-container">
                        <button class="edit-contact-button"  @click="setEditingContact(contact)">
                              <span class="edit-icon"></span>
                        </button>
                        <button class="delete-contact-button" @click="removeContact(contact.id)">
                              <span class="delete-icon"></span>
                        </button>
                  </div>
            </div>
            
            <!-- This div contains the contact details and is ONLY visible IF the user clicks the contact name (corresponding function and @click above)-->
            <div v-if="contact.showDetails">

               <!-- This template allows the user to edit a contact and is ONLY visible IF the user clicks on the "edit-contact-button" (corresponding function and @click above) -->
              <template v-if="editingContact && editingContact.id === contact.id">
                  <div class="edit-form">
                        <div class="edit-form">
                              <input v-model="editingContact.firstname" class="edit-form-input" type="text" placeholder="First Name" />
                              <input v-model="editingContact.lastname" class="edit-form-input" type="text" placeholder="Last Name" />
                              <input v-model="editingContact.phone" class="edit-form-input" type="text" placeholder="Phone Number" />
                              <input v-model="editingContact.email" class="edit-form-input" type="text" placeholder="Email" />
                              <button class="save-button" @click="saveEditedContact">Save</button> 
                              <button class="cancel-button" @click="cancelEdit">Cancel</button>
                        </div>
                  </div>
              </template>
              <p>Phone: {{ contact.phone }}</p>
              <p>Email: {{ contact.email }}</p>
            </div>
      </li>
    </ul>
  </div>
</template>

<!-- *************************************** MY CSS ****************************************** -->
<style scoped>
h3 {
      font-weight: 500;
      font-size: 1.5rem;
}
.contacts-container {
      background-color: rgb(212, 212, 212);
      margin-top: 20px;
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
.top-bar {
      display: flex;
      padding: 20px;
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

/* List of contacts */
.contacts-list {
      list-style-type: none;
      background-color: rgb(255, 255, 255);
      padding-top: 20px;
      padding-bottom: 10px;
}
.each-contact {
      display: flex;
      justify-content: space-between;
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

/* Edit Button */
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

/* Delete Button */
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

/* Hidden Edit Form */

.edit-form {
      background-color: rgb(216, 220, 213);
      margin-top: 15px;
      margin-right: 10px;
      margin-bottom: 15px;
      border-radius: 6px;
      padding: 5px;
      padding-left: 10px;
      display: flex;
}
.edit-form-input {
      width: 20%;
      border: none;
      padding-top: 10px;
      padding-bottom: 10px;
      border-radius: 6px;
      padding-left: 10px;
      margin-right: 10px;
      font-size: 15px;
}
.edit-form-input::placeholder {
      font-size: 15px;
      color: rgb(128, 128, 128);
      font-style: italic;
}
input:focus {
      outline: none;
}
.save-button {
      padding: 10px;
      margin-right: 10px;
      background-color: rgb(52, 145, 66);
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
}
.save-button:active {
      background-color: rgb(14, 67, 22);
}
.cancel-button {
      padding: 10px;
      background-color:rgb(142, 53, 53);
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
}
.cancel-button:active {
      background-color: rgb(95, 34, 34);
}

</style>
