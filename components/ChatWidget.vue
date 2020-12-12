<template>
  <v-card class="mx-auto chatWidget">
    <v-toolbar color="dark accent-4" dark>
      <v-toolbar-title>My Quiz</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-plus</v-icon>
      </v-btn>
    </v-toolbar>

    <v-list dense>
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title>Me</v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-list-item
        v-for="client in clients"
        :key="client.title"
      >
        <v-list-item-content>
          <v-list-item-title v-text="client.title"></v-list-item-title>
        </v-list-item-content>
        <v-btn icon>
          <v-icon>
            mdi-account-key
          </v-icon>
        </v-btn>
      </v-list-item>
    </v-list>

    <v-divider></v-divider>

    <v-card-text>

          <v-text-field
            disabled
            outlined
            clearable
            :value="roomUrl"
            type="text"
          >
            <template v-slot:append>
                <v-icon>
                  mdi-link
                </v-icon>
            </template>
          </v-text-field>

    </v-card-text>

    <v-divider></v-divider>

    <v-card-text class="chatWidget__messageWindow">
      <p class="chatWidget__messageText my-0" v-for="(message, index) in messages" :key="index">
        <strong>{{ message }}</strong>
      </p>
    </v-card-text>
    <v-card-actions>
      <v-text-field
        v-model="message"
        class="mx-2"
        label="Say something..."
        rows="1"
        prepend-icon="mdi-comment"
        @change="updateMessage"
        @keydown.enter="send"
      ></v-text-field>
      <v-btn @click="send">Send</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  data: () => ({
    message: '',
    clients: [],
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
    roomUrl: {
      type: String,
      required: true,
    },
  },
}
</script>

<style lang="scss" scoped>
.chatWidget {
  &__messageWindow {
    overflow-y: scroll;
    height: 200px;
  }
  &__messageText {
    font-size: 12px;
  }
}
</style>
