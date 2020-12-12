<template>
  <v-layout>
    <v-row justify-center align-center>
      <v-col class="col-12">
        <v-card class="mt-10">
          <v-card-title class="headline">
            QuizQuiz Vue
          </v-card-title>
          <v-card-actions>
            <v-btn
              color="primary"
              nuxt
              to="/play/"
              @click="joinQuizDialog = true"
            >
              Join
            </v-btn>
            <v-btn
              color="primary"
              nuxt
              @click="createQuiz"
            >
              Create
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

    <DialogSetName :isOpen="joinQuizDialog" @submit="joinQuiz" />

  </v-layout>
</template>

<script>
import DialogSetName from '~/components/DialogSetName'

export default {
  data: () => ({
    joinQuizDialog: false,
  }),
  components: {
    DialogSetName,
  },
  methods: {
    joinQuiz(name) {
      this.$router.push(`play/${name}`)
    },
    createQuiz() {
      this.$router.push('play')
    },
  },
}
</script>
