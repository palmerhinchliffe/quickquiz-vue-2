<template>
  <v-dialog
    v-model="isOpen"
    max-width="300"
  >
    <v-card>
      <v-card-title>
        Create a Quiz
      </v-card-title>
      <v-divider />
      <v-card-text>
        <v-text-field
          v-model.trim="$v.name.$model"
          :error-messages="validationErrorMessages"
          class="mt-5"
          label="Room Name"
          required
          @keyup="$v.$touch"
        ></v-text-field>
        <v-checkbox v-model="isPrivate">
          <template v-slot:label>
            <v-tooltip bottom>
              <template v-slot:activator="{ on }">
                  <span v-on="on">Private</span>
              </template>
              Only you and those you invite can join a private room.
            </v-tooltip>
          </template>
        </v-checkbox>
        <v-text-field
          v-model.trim="$v.name.$model"
          :error-messages="validationErrorMessages"
          :disabled="!this.isPrivate"
          label="Password"
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
    isPrivate: false,
    password: '',
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
      minLength: minLength(4),
      maxLength: maxLength(20),
    },
    password: {
      alphaNum,
    },
  },
}
</script>
