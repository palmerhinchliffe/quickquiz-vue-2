<template>
  <v-layout>
    <v-row justify-center>
      <v-col class="col-12 col-lg-9">
        <GameConfigWidget />
      </v-col>
      <v-col class="col-12 col-lg-3">
        <ChatWidget :messages="messages" @send="sendChatMessage" />
      </v-col>
    </v-row>
  </v-layout>
</template>

<script>
import * as Colyseus from 'colyseus.js'
import GameConfigWidget from '~/components/GameConfigWidget'
import ChatWidget from '~/components/ChatWidget'

export default {
  data: () => ({
    clients: [],
    messages: [],
    room: null,
    error: null,
  }),
  components: {
    GameConfigWidget,
    ChatWidget,
  },
  methods: {
    sendChatMessage(message) {
      this.room.send('message', message)
    },
  },
  mounted() {
    const client = new Colyseus.Client('ws://localhost:2567')

    client.joinOrCreate('quiz').then(room => {
      this.room = room

      // new room state
      room.onStateChange((state) => {
        // this signal is triggered on each patch
        console.log(state.players)
      })

      room.onError((code, message) => {
        console.log(client.id, 'couldn\'t join', room.name)
      })

      // listen to patches coming from the server
      room.onMessage('messages', (message) => {
        this.messages.push(message)
      })

      room.onLeave((code) => {
        console.log(client.id, 'left', room.name)
      })
    }).catch(error => {
        this.error = error
    })
  },
}
</script>
