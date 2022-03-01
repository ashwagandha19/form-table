<template>
    <ag-grid-vue style="height: 500px; width: 100%"
        class="ag-theme-alpine gridCont"
        :columnDefs="columnDefs"
        :rowData="rowData"
        rowSelection="multiple"
        >
    </ag-grid-vue>
</template>

<script>
import { AgGridVue } from "ag-grid-vue";

export default {
 name: "App",
 data() {
   return {
     columnDefs: null,
     rowData: null,
   };
 },
 components: {
   AgGridVue,
 },
 beforeMount() {
   this.columnDefs = [
        { field: 'userId', sortable: true, filter: true, checkboxSelection: true},
        { field: 'id', sortable: true, filter: true, },
       { field: 'title', sortable: true, filter: true },
       { field: 'completed', sortable: true, filter: true }
    ];

     fetch('https://jsonplaceholder.typicode.com/todos')
             .then(result => result.json())
             .then(rowData => this.rowData = rowData)
             .catch(err => console.log(err));
 },
};
</script>

<style scoped>
@import "~ag-grid-community/dist/styles/ag-grid.css";
@import "~ag-grid-community/dist/styles/ag-theme-alpine.css";

.gridCont {
    height: 100vh !important;
}
</style>