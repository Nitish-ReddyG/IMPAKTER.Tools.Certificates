<template>
  <div class="myDocuments">
    <b-table 
      class="docuTable"
      :items="items"
      :fields="fields"
      :select-mode="selectMode"
      responsive="sm"
      ref="selectableTable"
      selectable
      @row-selected="onRowSelected"
    >

      <template #cell(selected)="{ rowSelected }">
        <template v-if="rowSelected">
           <b-form-checkbox checked="true">
            </b-form-checkbox>
          <span class="sr-only">Selected</span>
        </template>
        <template v-else>
           <b-form-checkbox>
            </b-form-checkbox>
          <span aria-hidden="true">&nbsp;</span>
          <span class="sr-only">Not selected</span>
        </template>
      </template>
      
      <template #cell(action)>
        <b-button class="actionBtn"><b-icon icon="trash"></b-icon></b-button>
      </template>
    </b-table>
  </div>
</template>

<script>

export default {
   name: "MyDocuments",
   data() {
      return {
        fields: ['selected', 'documentName', 'link', 'action'],
        items: [
          {  documentName: "File Title example", link: 'http://wwww.impakter.com', },
          {  documentName: "File Title example", link: 'http://wwww.impakter.com', },
          {  documentName: "File Title example", link: 'http://wwww.impakter.com', },
          {  documentName: "File Title example", link: 'http://wwww.impakter.com',  }
        ],
        selectMode: 'multi',
        selected: []
      }
    },
    methods: {
      onRowSelected(items) {
        this.selected = items
      },
      selectAllRows() {
        this.$refs.selectableTable.selectAllRows()
      },
      clearSelected() {
        this.$refs.selectableTable.clearSelected()
      },
      selectThirdRow() {
        // Rows are indexed from 0, so the third row is index 2
        this.$refs.selectableTable.selectRow(2)
      },
      unselectThirdRow() {
        // Rows are indexed from 0, so the third row is index 2
        this.$refs.selectableTable.unselectRow(2)
      }
    }
}
</script>

<style scoped>

.myDocuments{
    text-align: left;
}
.docuTable{
  margin: -17px 0 0 0;
}

.actionBtn{
  color:black;
  border: transparent;
  background: white;
}
</style>