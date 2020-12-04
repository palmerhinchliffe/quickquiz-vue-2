<template>
  <v-card
    class="mx-auto"
  >
    <v-toolbar color="dark accent-4" dark>
      <v-toolbar-title>Cindy's Quiz</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-plus</v-icon>
      </v-btn>
    </v-toolbar>

    <v-list>
      <v-list-item
        v-for="client in clients"
        :key="client.title"
      >
        <v-list-item-avatar>
          <v-img
            :alt="`${client.title} avatar`"
            :src="client.avatar"
          ></v-img>
        </v-list-item-avatar>

        <v-list-item-content>
          <v-list-item-title v-text="client.title"></v-list-item-title>
        </v-list-item-content>
        <v-btn icon>
          <v-icon>
            mdi-account-remove
          </v-icon>
        </v-btn>
      </v-list-item>
    </v-list>

    <v-divider></v-divider>

    <v-card-text style="overflow-y: scroll; height: 150px;">
      <p v-for="(message, index) in messages" :key="index" style="font-size: 12px;">
        <strong>{{ message }}</strong>
      </p>
    </v-card-text>
    <v-card-actions>
      <v-textarea
        :value="message"
        class="mx-2"
        label="Say something..."
        rows="1"
        prepend-icon="mdi-comment"
        @change="updateMessage"
      ></v-textarea>
      <v-btn @click="send">Send</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    message: '',
    clients: [
      {
        active: true,
        avatar: 'https://cdn.vuetifyjs.com/images/lists/1.jpg',
        title: 'Jason Oner',
      },
      {
        active: true,
        avatar: 'https://cdn.vuetifyjs.com/images/lists/2.jpg',
        title: 'Mike Carlson',
      },
      {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/3.jpg',
        title: 'Cindy Baker',
      },
      {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/4.jpg',
        title: 'Ali Connors',
      },
    ],
  }),
  methods: {
    updateMessage(value) {
      this.message = value
    },
    send() {
      if (this.message.length) {
        this.$emit('send', this.message)
        this.message = ''
      }
    },
  },
  props: {
    messages: {
      type: Array,
      required: false,
    },
  },
}
</script>