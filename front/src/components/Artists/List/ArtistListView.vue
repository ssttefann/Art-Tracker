<template>
  <div class="mx-0 my-5">
    <v-data-table :headers="headers" :items="artists">
      <template v-slot:top>
        <v-toolbar flat color="white">
          <v-toolbar-title>Artists</v-toolbar-title>
        </v-toolbar>
      </template>
      <!-- <template v-slot:item.death="{ item }">
        <span v-if="item.death.day != ''">
          {{ item.death.year }}
        </span>
        <span v-else>Alive</span>
      </template> -->

      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)">
          mdi-pencil
        </v-icon>
        <v-icon small @click="deleteItem(item)">
          mdi-delete
        </v-icon>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import { mapMutations, mapGetters, mapActions } from 'vuex';
import { bus } from '@/main';

export default {
  data() {
    return {
      dialog: false,
      headers: [
        {
          text: 'Artist',
          align: 'start',
          value: 'name',
        },
        { text: 'Born', value: 'birthday.year' },
        { text: 'Place', value: 'birthplace' },
        { text: 'Died', value: 'death.year' },
        { text: 'Movement', value: 'artMovement' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
    };
  },

  computed: {
    ...mapGetters({
      artists: 'artists/getArtists',
    }),
  },

  methods: {
    ...mapMutations({
      openEditDialog: 'artistsDialog/openEditDialog',
      addEntry: 'autocomplete/addEntry',
    }),

    async editItem(item) {
      // add location to autocomplete entries
      await this.addEntry(item.birthplace);

      bus.$emit('openArtistDialog', {
        artist: item,
        type: 'edit',
        key: item['.key'],
      });
    },

    deleteItem(item) {
      bus.$emit('onDelete', {
        type: 'Artist',
        object: item,
        key: item['.key'],
      });
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    save() {
      if (this.editedIndex > -1) {
        Object.assign(this.artists[this.editedIndex], this.editedItem);
      } else {
        this.paintings.push(this.editedItem);
      }
      this.close();
    },
  },
};
</script>

<style></style>
