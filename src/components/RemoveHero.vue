<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <template v-slot:activator="{ on, attrs }">
      <v-btn dark text class="pink--text text--darken-1" v-bind="attrs" v-on="on">Remove</v-btn>
    </template>
    <v-alert class="red accent-2 white--text">
      <v-row align="center">
        <v-col
          class="font-weight-bold"
        >Are you sure you want to delete this hero? All data will be lost and cannot be recovered.</v-col>
        <v-col class="shrink">
          <v-btn text dark class="mb-4" @click="emitHero(hero.id)">Remove</v-btn>
          <v-btn text dark @click="dialog=false">Cancel</v-btn>
        </v-col>
      </v-row>
    </v-alert>
  </v-dialog>
</template>

<script>
export default {
  name: "RemoveHero",
  props: {
    hero: Object,
  },
  data: () => ({
    dialog: false,
  }),
  methods: {
    emitHero(id) {
      this.dialog = false;
      // emit hero id
      this.$emit("hero-emitted", id);
    },
  },
};
</script>