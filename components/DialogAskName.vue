<template>
  <v-dialog
    v-model="isOpen"
    persistent
    max-width="300"
  >
    <v-card>
      <v-card-text>
        <v-text-field
          v-model.trim="$v.name.$model"
          :error-messages="validationErrorMessages"
          class="mt-5"
          label="Name"
          required
          @keyup="$v.$touch"
        ></v-text-field>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          color="light"
          :disabled="$v.$invalid"
          @click="handleSubmit"
        >
          Go
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import { required, minLength, maxLength, alphaNum } from 'vuelidate/lib/validators'

export default {
  data: () => ({
    name: '',
  }),
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
  },
  computed: {
    validationErrorMessages() {
      let messages = []

      if (!this.$v.name.minLength) {
        messages.push('A bit longer than that...')
      }
      
      if (!this.$v.name.maxLength) {
        messages.push('Too Long!')
      }

      if (!this.$v.name.alphaNum) {
        messages.push('No special characters (%&><?/@...)')
      }

      return messages
    },
  },
  methods: {
    handleSubmit() {
      // Double check name valid in case user somehow submits without button
      if (!this.$v.$invalid) {
        this.$emit('submit', this.name)
      }
    },
  },
  validations: {
    name: {
      required,
      alphaNum,
      minLength: minLength(3),
      maxLength: maxLength(10),
    },
  },
}
</script>
