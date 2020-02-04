<template>
  <div class="container">
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group
        id="input-group-1"
        label="Email address:"
        label-for="input-1"
        description="We'll never share your email with anyone else."
      >
        <b-form-input
          id="input-1"
          v-model="form.email"
          type="email"
          required
          placeholder="Enter email"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-6" label="Password" label-for="input-6">
        <b-form-input
          id="input-6"
          v-model="form.password"
          type="password"
          required
          placeholder="Enter password"
        ></b-form-input>
      </b-form-group>

      <b-form-group
        id="input-group-2"
        label="Your firstname:"
        label-for="input-2"
      >
        <b-form-input
          id="input-2"
          v-model="form.firstname"
          required
          placeholder="Enter firstname"
        ></b-form-input>
      </b-form-group>

      <b-form-group
        id="input-group-5"
        label="Your lastname:"
        label-for="input-5"
      >
        <b-form-input
          id="input-5"
          v-model="form.lastname"
          required
          placeholder="Enter lastname"
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3" label="Roles:" label-for="input-3">
        <b-form-checkbox-group id="input-3" v-model="form.roles">
          <b-form-checkbox value="admin">admin</b-form-checkbox>
          <b-form-checkbox value="user">user</b-form-checkbox>
        </b-form-checkbox-group>
      </b-form-group>

      <b-form-group id="input-group-4">
        <b-form-select id="checkboxes-4" v-model="form.gender">
          <b-form-select-option value="m">male</b-form-select-option>
          <b-form-select-option value="f">female</b-form-select-option>
        </b-form-select>
      </b-form-group>

      <b-button type="submit" variant="primary">Submit</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
    <b-card class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
    </b-card>
  </div>
</template>

<script>
const apiUrl = 'localhost:8000/api/users'
export default {
  data() {
    return {
      form: {
        email: '',
        firstname: '',
        lastname: '',
        password: '',
        roles: null
      },
      roles: ['admin', 'user'],
      show: true
    }
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault()
      fetch(apiUrl, {
        method: 'POST',
        body: new URLSearchParams(this.form)
      })
    },
    onReset(evt) {
      evt.preventDefault()
      // Reset our form values
      this.form.email = ''
      this.form.firstname = ''
      this.form.lastname = ''
      this.form.roles = []
      this.form.gender = null
      // Trick to reset/clear native browser form validation state
      this.show = false
      this.$nextTick(() => {
        this.show = true
      })
    }
  }
}
</script>
