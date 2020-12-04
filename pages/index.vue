<template>
  <v-layout>
    <v-row justify-center align-center>
      <v-col class="col-12">
        <v-card class="mt-10">
          <v-card-title class="headline">
            Welcome to QuizQuiz Vue
          </v-card-title>
          <v-card-text>
            <p>This is a great app</p>
            <p>"Cick the button to start a quiz."</p>
            <div class="text-xs-right">
              <em><small>&mdash; Ghandi</small></em>
            </div>
          </v-card-text>
          <v-card-actions>
            <v-spacer />
            <v-btn
              color="primary"
              nuxt
              to="/play"
            >
              Quiz now
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-layout>
</template>

<script>
export default {
  
}
</script>
