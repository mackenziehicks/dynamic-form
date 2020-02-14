<template>
  <div class="field">
    <b-form-group
        v-if="field && conditional"
      >
        <div
          v-if="field.type === 'checkbox'"
          class="d-flex align-items-center"
        >
          <b-form-checkbox
            :id="field.name"
            :type="field.type"
            v-model="value"
          >
          </b-form-checkbox>
          <label
            :for="field.name"
            class="text-muted mb-1"
          >
            {{ field.human_label }}
          </label>
        </div>
        <div v-else>
          <label
          :for="field.name"
          class="text-muted mb-1"
        >
          {{ field.human_label }}
        </label>
         <b-form-input
            :id="field.name"
            :type="field.type"
            v-model="value"
            required
          >
          </b-form-input>
        </div>
      </b-form-group>
  </div>
</template>

<script>
export default {
  name: 'Field',
  props: {
    field: {
      type: Object,
      required: true,
    },
    conditional: {
      type: Boolean,
      required: false,
      default: true,
    },
  },
  data() {
    return {
      name: this.field.name,
      value: null,
    };
  },
  watch: {
    value() {
      this.$emit('userInput', { name: this.name, value: this.value });
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
label {
  font-size: 12px;
}
</style>
