<template>
  <div class="form">
    <div
      v-if="formData.length"
      class="align-center"
    >
      <b-card class="shadow">
        <b-form @submit.stop.prevent="onSubmit" @reset="onReset">
          <Field
            v-for="field in formData"
            :key="field.name"
            :field="field"
            @userInput="inputHandler"
            :conditional="conditional(field)"
          />
          <div class="text-center pt-3">
            <b-button
              type="submit"
              variant="info"
              class="mx-3"
            >
              Submit
            </b-button>
            <b-button
              type="reset"
              variant="outline-secondary"
              class="mx-3"
            >
              Reset
            </b-button>
          </div>
        </b-form>
      </b-card>
    </div>
  </div>
</template>

<script>
import Field from './Field.vue';

export default {
  name: 'Form',
  components: {
    Field,
  },
  props: {
    formData: {
      type: Array,
      required: true,
    },
    conditionalName: {
      type: String,
      required: false,
      default: null,
    },
    showIf: {
      type: Function,
      required: false,
      default: null,
    },
  },
  data() {
    return {
      userData: {},
    };
  },
  methods: {
    inputHandler(val) {
      if (this.userData[val.name]) {
        // If the userData object already has this key, set the value
        this.userData[val.name] = val.value;
      } else {
        // Otherwise, add the key using `.$set` so that it will be reactive
        this.$set(this.userData, val.name, val.value);
      }
    },
    onSubmit() {
      this.$emit('formSubmit', this.userData);
    },
    onReset() {
      this.userData = {};
      this.$emit('reset');
    },
    conditional(field) {
      if (field.conditional) {
        if (this.userData[this.conditionalName]) {
          return this.showIf(new Date(this.userData[this.conditionalName]));
        }
        return false;
      }
      return true;
    },
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.form {
  max-width: 500px;
}
.btn {
  min-width: 85px;
}
</style>
