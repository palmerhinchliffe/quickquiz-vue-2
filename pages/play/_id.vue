<template>
  <v-layout>
    <v-row justify-center>
      <v-col class="col-12 col-lg-9">
        <GameConfigWidget />
      </v-col>
      <v-col class="col-12 col-lg-3">
        <ChatWidget
          :messages="messages"
          :room-url="`http://localhost:3000/play/${this.room.id}`"
          @send="sendChatMessage"
        />
      </v-col>
    </v-row>

    <DialogSetName :is-open="dialogSetName" @submit="setPlayerName" />

    <DialogError :is-open="dialogError" :error="errorString" />
  </v-layout>
</template>

<script>
import * as Colyseus from 'colyseus.js'
import GameConfigWidget from '~/components/GameConfigWidget'
import ChatWidget from '~/components/ChatWidget'
import DialogSetName from '~/components/DialogSetName'

export default {
  data: () => ({
    dialogSetName: false,
    messages: [],
    room: {},
    dialogError: false,
    errorString: '',
  }),
  components: {
    GameConfigWidget,
    ChatWidget,
    DialogSetName,
  },
  methods: {
    sendChatMessage(message) {
      this.room.send('message', message)
    },
    setPlayerName(name) {
      this.room.send('setPlayerName', name)
      this.dialogSetName = false
    },
    onJoinOrCreate(room) {
      this.room = room
      console.log(room)
      this.dialogSetName = true

      // new room state
      room.onStateChange((state) => {
        // this signal is triggered on each patch
      })

      room.onError((code, message) => {
        console.log(client.id, 'couldn\'t join', room.name)
      })

      // listen to patches coming from the server
      room.onMessage('messages', (message) => {
        this.messages.push(message)
      })

      room.onLeave((code) => {
      })
    }
  },
  mounted() {
    const client = new Colyseus.Client('ws://localhost:2567')
    let quizId

    if (this.$route.params.id) {
      // Attempt to join existing quiz
      client.joinById(this.$route.params.id).then(room => {
        this.onJoinOrCreate(room)
      }).catch(error => {
          this.error = error
          this.dialogError = true
      })
    } else {
      client.create('quiz').then(room => {
        this.onJoinOrCreate(room)
      }).catch(error => {
          this.error = error
          this.dialogError = true
      })
    }
  },
}
</script>
