<template>
  <v-container class="top-cont px-6 pt-2 content-color">
    <v-row class="mr-3 ml-5">
      <v-spacer></v-spacer>
      <v-btn @click="addPainting" class="mx-5 text-none blue--text" tile>
        <v-icon class="mr-5 black--text">mdi-plus</v-icon>
        Add Painting
      </v-btn>
      <v-btn-toggle v-model="selectedView" mandatory dense borderless>
        <v-btn icon><v-icon>mdi-view-grid</v-icon></v-btn>
        <v-btn icon><v-icon>mdi-format-list-bulleted</v-icon></v-btn>
      </v-btn-toggle>
    </v-row>

    <PaintingGridView class="mt-12" :hidden="selectedView == 1" />
    <PaintingListView :hidden="selectedView == 0" />

    <PaintingsDialog />
  </v-container>
</template>

<script>
import { mapMutations, mapGetters } from 'vuex';
import PaintingsDialog from './Dialogs/PaintingsDialog';
import PaintingGridView from './Grid/PaintingGridView';
import PaintingListView from './List/PaintingListView';
import { bus } from '@/main';

export default {
  components: {
    PaintingsDialog,
    PaintingGridView,
    PaintingListView,
  },

  data() {
    return {
      selectedView: '',
    };
  },

  methods: {
    addPainting() {
      bus.$emit('openPaintingDialog', {
        painting: '',
        type: 'add',
        key: '',
      });
    },
  },

  computed: {
    ...mapGetters({
      artMovements: 'paintings/getArtMovements',
    }),
  },
};
</script>

<style scoped>
.top-cont {
  height: 100%;
}
</style>
