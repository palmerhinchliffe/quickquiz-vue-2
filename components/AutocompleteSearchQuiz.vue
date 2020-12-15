<template>
  <div>
    <v-autocomplete
      v-model="model"
      :items="items"
      :loading="isLoading"
      :search-input.sync="search"
      color="white"
      hide-no-data
      hide-selected
      item-text="Description"
      item-value="API"
      label="Search via name"
      placeholder="Search..."
      prepend-icon="mdi-database-search"
      return-object
    ></v-autocomplete>
    <v-divider></v-divider>
    <v-expand-transition>
      <v-list
        v-if="model"
        class="light"
      >
        <v-list-item
          v-for="(field, i) in fields"
          :key="i"
        >
          <v-list-item-content>
            <v-list-item-subtitle v-text="field.key"></v-list-item-subtitle>
            <v-list-item-title v-text="field.value"></v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-expand-transition>
  </div>
</template>

<script>
export default {
  data: () => ({
    descriptionLimit: 60,
    entries: [],
    isLoading: false,
    model: null,
    search: null,
  }),
  mounted() {
    const client = new Colyseus.Client('ws://localhost:2567')
    
    client.getAvailableRooms("quiz").then(rooms => {
      rooms.forEach((room) => {
        console.log(room.roomId)
        console.log(room.clients)
        console.log(room.maxClients)
        console.log(room.metadata)
      })
    }).catch(e => {
      console.error(e);
    })
  },
  computed: {
    fields () {
      if (!this.model) return []

      return Object.keys(this.model).map(key => {
        return {
          key,
          value: this.model[key] || 'n/a',
        }
      })
    },
    items () {
      return this.entries.map(entry => {
        const Description = entry.Description.length > this.descriptionLimit
          ? entry.Description.slice(0, this.descriptionLimit) + '...'
          : entry.Description

        return Object.assign({}, entry, { Description })
      })
    },
  },

  watch: {
    search (val) {
      // Items have already been loaded
      if (this.items.length > 0) return

      // Items have already been requested
      if (this.isLoading) return

      this.isLoading = true

      // Lazily load input items
      fetch('https://api.publicapis.org/entries')
        .then(res => res.json())
        .then(res => {
          const { count, entries } = res
          this.count = count
          this.entries = entries
        })
        .catch(err => {
          console.log(err)
        })
        .finally(() => (this.isLoading = false))
    },
  },
}
</script>