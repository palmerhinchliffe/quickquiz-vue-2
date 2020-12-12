<template>
  <v-dialog
    v-model="isOpen"
    persistant
    max-width="300"
  >
    <v-card>
      <v-card-text>
        {{ error }}
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          color="light"
        >
          OK
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>

export default {
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
    error: {
      type: String,
      required: false,
      default: 'Oops there was an error.',
    },
  },
}
</script>
