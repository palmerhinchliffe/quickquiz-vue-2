<template>
  <v-layout>
    <v-row class="mt-10">
      <v-col class="col-12 col-md-6">
        <v-card>
          <v-card-title>
            Create a Quiz
          </v-card-title>
          <v-card-text>
            Create a quiz and invite your friends
          </v-card-text>
          <v-card-actions>
            <v-btn
              color="primary"
              block
              @click="createQuizDialog = true"
            >
              Create
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      <v-col class="col-12 col-md-6">
        <v-card>
          <v-card-title>
            Join a Friend's Quiz
          </v-card-title>
          <v-card-text>
            Join an existing quiz using an ID send from a friend.
          </v-card-text>
          <v-card-actions>
            <v-btn
              color="primary"
              block
              @click="joinQuizDialog = true"
            >
              Join
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
      <v-col class="col-12">
        <v-list
          subheader
          two-line
        >

          <v-list-item
            v-for="room in availableRooms"
            :key="room.id"
          >
            <v-list-item-avatar>
              <v-icon
                class="grey lighten-1"
              >
                mdi-users
              </v-icon>
            </v-list-item-avatar>

            <v-list-item-content>
              <v-list-item-title>
                {{ room.roomId }}
              </v-list-item-title>

              <v-list-item-subtitle>
                {{ room.clients }}/{{ room.maxClients ? room.maxClients : '&#8734;' }}
              </v-list-item-subtitle>
            </v-list-item-content>

            <v-list-item-action>
              <v-btn icon>
                <v-icon color="error lighten-2">mdi-key</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-col>
    </v-row>

    <DialogCreateQuiz :isOpen="createQuizDialog" @submit="createQuiz" />
    <DialogJoinQuiz :isOpen="joinQuizDialog" @submit="joinQuiz" />

  </v-layout>
</template>

<script>
import * as Colyseus from 'colyseus.js'
import DialogJoinQuiz from  '~/components/DialogJoinQuiz'
import DialogCreateQuiz from '~/components/DialogCreateQuiz'

export default {
  data: () => ({
    joinQuizDialog: false,
    createQuizDialog: false,
    availableRooms: [],
  }),
  async mounted() {
    const client = new Colyseus.Client('ws://localhost:2567')

    client.getAvailableRooms('quiz').then((rooms) => {
      console.log(rooms)
      this.availableRooms = rooms
    }).catch((error) => {
      console.log(error)
    })
  },
  components: {
    DialogJoinQuiz,
    DialogCreateQuiz,
  },
  methods: {
    joinQuiz(name) {
      this.$router.push(`play/${name}`)
    },
    createQuiz(options) {
      this.$router.push('play')
    },
  },
}
</script>
