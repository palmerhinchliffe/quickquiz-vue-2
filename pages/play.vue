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

    <DialogAskName :isOpen="!hasGivenName" @submit="setPlayerName" />
  </v-layout>
</template>

<script>
import * as Colyseus from 'colyseus.js'
import GameConfigWidget from '~/components/GameConfigWidget'
import ChatWidget from '~/components/ChatWidget'
import DialogAskName from '~/components/DialogAskName'

export default {
  data: () => ({
    hasGivenName: false,
    messages: [],
    room: {},
    error: '',
  }),
  components: {
    GameConfigWidget,
    ChatWidget,
    DialogAskName,
  },
  methods: {
    sendChatMessage(message) {
      this.room.send('message', message)
    },
    setPlayerName(name) {
      this.room.send('setPlayerName', name)
      this.hasGivenName = true
    },
  },
  mounted() {
    const client = new Colyseus.Client('ws://localhost:2567')

    client.joinOrCreate('quiz').then(room => {
      this.room = room

      // new room state
      room.onStateChange((state) => {
        // this signal is triggered on each patch
        console.log(state)
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
    }).catch(error => {
        this.error = error
    })
  },
}
</script>
