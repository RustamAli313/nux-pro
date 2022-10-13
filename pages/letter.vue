<template>
  <div>
    <template>
      <div class="text-center">
        <v-dialog v-model="dialog" width="500">
          <v-card>
            <v-card-title class="text-h5 grey lighten-2">
              Privacy Policy
            </v-card-title>

            <v-card-text>
              <v-row>
                <v-col cols="12">
                  <v-text-field label="Name" v-model="name"> </v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-textarea label="Content" v-model="content"> </v-textarea>
                </v-col>
              </v-row>
            </v-card-text>

            <v-divider></v-divider>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" text :loading="load" @click="submit">
                {{ isEdited ? "Update" : "Save" }}
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </div>
    </template>

    <Datatable
      :headers="header"
      :items="letters"
      :loading="loading"
      @getRecord="getData"
      @insertModel="insertModel"
      @editModel="editItem"
      @deleteItem="deleteItem"
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
      content: "",
      id: "",
      isEdited: false,
      header: [
        { text: "ID", value: "id" },
        { text: "Name", value: "name" },
        { text: "Content", value: "content" },
        { text: "Status", value: "status" },
        { text: "Actions", value: "actions" },
      ],
      letters: [],
    };
  },
  created() {
    this.getData();
  },
  methods: {
    insertModel(){
      this.dialog=true;
    },
    async getData(item=null) {
      this.loading=true;
      const result = await this.$axios.get("letter",{params:{
        tabKey: item,
      }});
      this.letters = result.data;
      this.loading=false;
    },
    async submit() {
      this.load = true;
      let info = {
        name: this.name,
        content: this.content,
      };
      if (this.isEdited) {
        this.update(info);
      } else {
        const store = await this.$axios.post("letter", info);
        if (store.status == 201) {
          this.letters.push(store.data);
        }
      }
      this.load = false;
      this.dialog = false;
      this.reset();
    },
    async deleteItem(data) {

      const dell = await this.$axios.delete(`letter/id`,{params:data});
      if (dell.status == 200) {
       this.load=true;
       await this.getData(data.tabKey);
       this.load=false;
      }
      this.reset();

    },
    editItem(item) {
      this.id = item.id;
      this.name =item.name ;
      this.content = item.content;
      this.isEdited = true;
      this.dialog = true;
    },
    async update(info) {
      const update = await this.$axios.put(`letter/${this.id}`, info);
      if (update.status == 200) {
        this.letters = this.letters.map((item) => {
          if (item.id == this.id) {
            return update.data;
          } else {
            return item;
          }
        });
      }
      this.reset();
    },
    reset(){
      this.name='';
      this.content='';

    },
  },
};
</script>
