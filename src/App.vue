<template>
  <div
    id="app"
    class="container my-5"
  >
    <div class="mb-4 text-center">
      <img
        alt="Vue logo"
        src="./assets/sparrow.png"
      >
      <h1 class="mb-0 px-1">User Form</h1>
    </div>
    <div class="d-flex">
      <b-col>
        <Form
          :formData="formData"
          :conditionalName="conditionalName"
          :showIf="conditionalShowIf"
          @formSubmit="submit"
          @reset="reset"
        />
      </b-col>
      <b-col>
        <b-card class="reult-card shadow">
          <h1 class="h5">Result: </h1>
          <p>{{ user }}</p>
        </b-card>
      </b-col>
    </div>
  </div>
</template>

<script>
import Form from './components/Form.vue';

export default {
  name: 'App',
  components: {
    Form,
  },
  data() {
    return {
      user: {},
      conditionalName: null,
      conditionalShowIf: null,
      formData: [
        {
          "tag": "input",
          "name": "first_name",
          "type": "text",
          "human_label": "First Name"
        }, {
          "tag": "input",
          "name": "last_name",
          "type": "text",
          "human_label": "Last Name"
        }, {
          "tag": "input",
          "name": "email",
          "type": "email",
          "human_label": "Email Address"
        }, {
          "tag": "input",
          "name": "phone_number",
          "type": "text",
          "human_label": "Phone Number"
        }, {
          "tag": "input",
          "name": "job_title",
          "type": "text",
          "human_label": "Job Title"
        }, {
          "tag": "input",
          "name": "date_of_birth",
          "type": "date",
          "human_label": "Date of Birth"
        }, {
          "tag": "input",
          "name": "parental_consent",
          "type": "checkbox",
          "human_label": "Parental Consent",
          "conditional": {
            "name": "date_of_birth",
            "show_if": (value) => {
              const now = new Date();
              return value >= new Date(now.getFullYear() - 13, now.getMonth(), now.getDate());
            }
          }
        },
      ],
    };
  },
  methods: {
    submit(data) {
      this.user = data;
    },
    reset() {
      this.user = {};
    },
  },
  mounted() {
    // I ran into an issue where the function in the "conditional"
    // object was not being passed to another component unless
    // it was passed directly with the propType: Function.  To get arount that,
    // I added this to pull out the "show_if" function "name" and pass them directly to the
    // form component. This obviously does not account for multiple conditional fields,
    // ideally the function would be able to be passed within the formData object.
    this.formData.forEach((field) => {
      if (field.conditional) {
        this.conditionalName = field.conditional.name;
        this.conditionalShowIf = field.conditional.show_if;
      }
    });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
img {
  height: 75px;
}
</style>
