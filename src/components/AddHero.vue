<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <template v-slot:activator="{ on, attrs }">
      <v-btn dark v-bind="attrs" v-on="on" class="py-6 green--text text--accent-3">Add Hero</v-btn>
    </template>
    <v-card>
      <v-card-title class="green accent-4">
        <span class="headline white--text">Add Hero</span>
      </v-card-title>
      <v-card-text>
        <v-form ref="form">
          <v-row>
            <v-col cols="12" sm="6" md="4">
              <v-text-field
                label="Hero Name*"
                :rules="nameRules"
                counter
                maxlength="30"
                required
                v-model="name"
              ></v-text-field>
            </v-col>
            <v-col cols="12" sm="6" md="4">
              <v-text-field label="Year" maxlength="4" :rules="yearRules" v-model="year"></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field label="Info*" counter required :rules="infoRules" v-model="info"></v-text-field>
            </v-col>
          </v-row>
        </v-form>
        <small>*indicates required field</small>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="green darken-1" text @click="dialog = false">Cancel</v-btn>
        <v-btn color="green darken-1" text @click="validate">Add</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "AddHero",
  props: {
    nameRules: Array,
    infoRules: Array,
    yearRules: Array,
  },
  data: () => ({
    dialog: false,
    name: "",
    info: "",
    year: null,
  }),
  methods: {
    emitHero() {
      this.dialog = false;

      // create new hero
      const data = {};
      data["name"] = this.name;
      data["info"] = this.info;
      // optional fields
      if (this.year) {
        data["year"] = parseInt(this.year);
      }

      this.$emit("hero-emitted", data);
    },
    validate() {
      if (this.$refs.form.validate()) {
        this.emitHero();
        this.$refs.form.reset();
      }
    },
  },
};
</script>

<style>
</style>