<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 text-success fw-bold">
          Contact Manager
          <RouterLink to="/contacts/add" class="btn btn-success btn-sm">
            <i class="fa fa-plus-circle"></i> New</RouterLink
          >
        </p>
        <p class="fst-italic">
          Lorem ipsum dolor sit amet consectetur adipisicing elit. Officiis voluptate, incidunt
          laborum maxime itaque quaerat fuga hic, nemo animi non voluptates sint, perspiciatis
          maiores voluptas nisi iure! Ipsum, molestiae ratione?
        </p>
        <form>
          <div class="row">
            <div class="col-md-6">
              <div class="row">
                <div class="col">
                  <input type="text" class="form-control" placeholder="Search Name" />
                </div>
                <div class="col">
                  <input type="submit" class="btn btn-outline-dark" />
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>

  <!-- Spinner -->
  <div v-if="loading">
    <div class="container">
      <div class="row">
        <div class="col">
          <Spinner />
        </div>
      </div>
    </div>
  </div>

  <!-- Error Message -->
  <div v-if="!loading && errorMessage">
    <div class="container mt-3">
      <div class="row">
        <div class="col">
          <p class="h4 text-danger fw-bold">{{ errorMessage }}</p>
        </div>
      </div>
    </div>
  </div>

  <div class="container mt-3" v-if="contacts.length > 0">
    <div class="row">
      <div class="col-md-6" v-for="contact of contacts" :key="contact">
        <div class="card my-2 bg-light shadow-lg">
          <div class="card-body">
            <div class="row align-items-center">
              <div class="col-sm-4">
                <img :src="contact.photo" alt="Wait" class="contact-img" />
              </div>
              <div class="col-sm-7">
                <ul class="list-group">
                  <li class="list-group-item">
                    Name: <span class="fw-bold">{{ contact.name }}</span>
                  </li>
                  <li class="list-group-item">
                    Email: <span class="fw-bold">{{ contact.email }}</span>
                  </li>
                  <li class="list-group-item">
                    Mobile: <span class="fw-bold">{{ contact.mobile }}</span>
                  </li>
                </ul>
              </div>
              <div class="col-sm-1 d-flex flex-column justify-content-center align-items-center">
                <RouterLink :to="`/contacts/view/${contact.id}`" class="btn btn-warning my-1">
                  <i class="fa fa-eye"></i>
                </RouterLink>
                <RouterLink :to="`/contacts/edit/${contact.id}`" class="btn btn-primary my-1">
                  <i class="fa fa-pen"></i>
                </RouterLink>
                <button class="btn btn-danger my-1" @click="clickDeleteContact(contact.id)">
                  <i class="fa fa-trash"></i>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Spinner from '../components/Spinner.vue'
import { ContactService } from '../services/ContactService'

export default {
  name: 'ContactManager',
  data: function () {
    return {
      loading: false,
      contacts: [],
      errorMessage: null
    }
  },
  created: async function () {
    try {
      this.loading = true
      let response = await ContactService.getAllContacts()
      this.contacts = response.data
      this.loading = false
    } catch (error) {
      this.errorMessage = error
      this.loading = false
    }
  },
  methods: {
    clickDeleteContact: async function (contactId) {
      try {
        this.loading = true
        let response = await ContactService.deleteContact(contactId)
        if (response) {
          let response = await ContactService.getAllContacts()
          this.contacts = response.data
          this.loading = false
        }
      } catch (error) {
        this.errorMessage = error
        this.loading = false
      }
    },
    getAllContactsData: async function () {
      return await ContactService.getAllContacts()
    }
  },
  components: { Spinner }
}
</script>

<style></style>
