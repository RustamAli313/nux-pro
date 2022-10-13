<template>
  <div>
    <template>
      <div class="text-center">
        <v-dialog v-model="dialog" width="500">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="red lighten-2" dark v-bind="attrs" v-on="on">
              Click Me
            </v-btn>
          </template>

          <v-card>
            <v-card-title class="text-h5 grey lighten-2">
              Privacy Policy
            </v-card-title>

            <v-card-text>
              <v-row>
                <v-col cols="12">
                  <v-text-field v-model="name" label="Name"></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field v-model="author" label="Author"> </v-text-field>
                </v-col>
              </v-row>
            </v-card-text>

            <v-divider></v-divider>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" text :loading="load" @click="submit">
                {{ isEdit ? "Update" : "Save" }}
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
    </template>
    <Datatable
      :headers="header"
      :items="books"
      :tabItems="tabItems"
      :loading="loading"
    >

    </Datatable>
  </div>
</template>


<script>
import Datatable from "~/components/common/datatable.vue";

export default {
  components: { Datatable },
  data() {
    return {
      load: false,
      loading:false,
      dialog: false,
      name: "",
      author: "",
      id: "",
      isEdit: false,
      header: [
        { text: "ID", value: "id" },
        { text: "Name", value: "name" },
        { text: "Author", value: "author" },
        { text: "Actions", value: "actions" },
      ],

      books: [],
    };
  },
  created() {
    this.getData();
  },
  methods: {
    async getData() {
      loading=true;
      const result = await this.$axios.get("book");
      this.books = result.data;
      loading=false;
    },
    async submit() {
      this.load = true;
      let info = {
        name: this.name,
        author: this.author,
      };
      if (this.isEdit) {
        this.update(info);
      } else {
        const store = await this.$axios.post("book", info);
        if (store.status == 201) {
          this.books.push(store.data);
        }
      }
      this.load = false;
      this.dialog = false;
    },
    async deleteItem(id) {
      const dell = await this.$axios.delete(`book/${id}`);
      if (dell.status == 200) {
        this.books = this.books.filter((item) => item.id != id);
      }
    },
    async editItem(item) {
      this.id = item.id;
      this.name = item.name;
      this.author = item.author;
      this.isEdit = true;
      this.dialog = true;
    },
    async update(info) {
      this.isEdit = false;
      const update = await this.$axios.put(`book/${this.id}`, info);
      if (update.status == 200) {
        this.books = this.books.map((item) => {
          if (item.id == this.id) {
            return update.data;
          } else {
            return item;
          }
        });
      }
      this.reset();
    },
    reset() {
      this.name = "";
      this.author = "";
      this.id = "";
    },
  },
};
</script>
