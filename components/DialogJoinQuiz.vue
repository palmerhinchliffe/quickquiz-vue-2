<template>
  <v-dialog
    v-model="isOpen"
    max-width="300"
  >
    <v-card>
      <v-card-title>
        Join a Quiz
      </v-card-title>
      <v-divider />
      <v-card-text>
        <AutocompleteSearchQuiz class="my-5" />
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
import AutocompleteSearchQuiz from '~/components/AutocompleteSearchQuiz'

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
  components: {
    AutocompleteSearchQuiz,
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
