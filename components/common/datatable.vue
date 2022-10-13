<template>
  <div>
    <template>
      <v-card class="elevation-1 my-2 py-2 text-center">
        <v-btn class="btn success" @click="editModel" :class="selected.length>0?'':'customDisabled'" >Edit</v-btn>
        <v-btn class="btn primary " v-if="tabKey == 'remove'" @click="deleted('restore')" :class="selected.length>0?'':'customDisabled'"  >Restore</v-btn>
        <v-btn class="btn red lighten-2" style="color:white;"  @click="deleted('delete')" :class="selected.length>0?'':'customDisabled'"   >Delete</v-btn>
        <v-btn class="btn info" @click="openModel" >Insert</v-btn>

      </v-card>
      <v-card>
        <v-tabs  background-color="primary" dark>
          <v-tab v-for="item in tabItems" :key="item.tab" @click="tabChange(item.value)">
              <v-icon>{{item.icon}}</v-icon>  {{ item.tab }}
          </v-tab>
        </v-tabs>

        <v-tabs-items >

          <v-data-table
            v-model="selected"
            :headers="headers"
            :items="items"
            item-key="id"
            show-select
            class="elevation-1"
            :loading="loading"
            @click:row="selectRow"
          >
          </v-data-table>
        </v-tabs-items>
      </v-card>
    </template>
  </div>
</template>




<script>
export default {
  props: {
    headers: Array,
    items: Array,
    loading:Boolean,


  },
  data() {
    return {
      tabKey:'all',
      selected:[],
      tabItems: [
        { tab: "ALL", value:'all' , icon:'mdi-account' },
        { tab: "ACTIVE", value:'active', icon:'mdi-account' },
        { tab: "INACTIVE", value:'inactive', icon:'mdi-account' },
        { tab: "REMOVE", value:'remove', icon:'mdi-account' },

      ],
    };
  },
  methods:{
    deleted(type){
      if(this.tabKey == 'all'){
        alert('You cannot delete when All tab is selected!');
      }else{
        let delId = this.selected.map((item)=>item.id);
        let data = {
          ids : delId,
          tabKey:this.tabKey,
          type: type
        };
        this.$emit('deleteItem',data);
        
      }

    },
    editModel(){
      if(this.selected.length==1){
      this.$emit('editModel',this.selected[0]);
      this.selected=[];
    }
      else
      alert("Please select one record!");

    },
    openModel(){
      this.$emit('insertModel');

    },
    tabChange(item){
      this.selected=[];
      this.tabKey = item;
      this.$emit('getRecord',item);

    },
    selectRow(item){
      if(this.selected.find((i)=>i.id == item.id)){
        this.selected = this.selected.filter((i)=>i.id != item.id);
      }else{
      this.selected.push(item);
    }
    }
  }
};
</script>

<style>
.customDisabled{
  opacity: 0.5;
  pointer-events: none !important;
  cursor: not-allowed !important;
}
</style>
