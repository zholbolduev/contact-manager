<template>
  <div class="container mt-3">
    <div class="row">
      <div class="col">
        <p class="h3 text-success fw-bold">View Contact</p>
        <p class="fst-italic">
          Lorem ipsum dolor sit amet consectetur, adipisicing elit. Facere odio totam saepe deleniti
          minima neque fugit illum ipsum ipsam necessitatibus nisi accusamus reiciendis blanditiis
          culpa, similique molestiae? Nemo, sunt libero.
        </p>
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

  <div class="container" v-if="!loading && isDone()">
    <div class="row align-items-center">
      <div class="col-md-4">
        <img :src="contact.photo" alt="Wait" class="contact-img-big" />
      </div>
      <div class="col-md-6">
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
          <li class="list-group-item">
            Company: <span class="fw-bold">{{ contact.company }}</span>
          </li>
          <li class="list-group-item">
            Title: <span class="fw-bold">{{ contact.title }}</span>
          </li>
          <li class="list-group-item">
            Group: <span class="fw-bold">{{ group.name }}</span>
          </li>
        </ul>
      </div>
    </div>

    <div class="row mt-3">
      <div class="col">
        <RouterLink to="/" class="btn btn-success"
          ><i class="fa fa-arrow-alt-circle-left"></i> Back</RouterLink
        >
      </div>
    </div>
  </div>
</template>

<script>
import Spinner from '../components/Spinner.vue'
import { ContactService } from '../services/ContactService'

export default {
  name: 'ViewContact',
  data: function () {
    return {
      contactId: this.$route.params.contactId,
      loading: false,
      contact: {},
      errorMessage: null,
      group: {}
    }
  },
  created: async function () {
    try {
      this.loading = true
      let response = await ContactService.getContact(this.contactId)
      let groupResponse = await ContactService.getGroup(response.data)
      this.contact = response.data
      this.group = groupResponse.data
      this.loading = false
    } catch (error) {
      this.errorMessage = error
      this.loading = false
    }
  },
  methods: {
    isDone: function () {
      return Object.keys(this.contact).length > 0 && Object.keys(this.group).length > 0
    }
  },
  components: { Spinner }
}
</script>

<style></style>
