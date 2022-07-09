<template>
  <tbody>
    <tr v-for="dataItem of dataList" :key="dataItem">
      <td class="name-cell">
        <span
          @click="editCell(dataItem.id)"
          v-show="!dataItem.isEditing">
            {{dataItem.name}}
        </span>
        <input class="inputEdit" v-show="dataItem.isEditing" v-on:keyup.enter="saveCell(dataItem.id)" v-model="inputData">
      </td>
      <td>{{dataItem.address}}</td>
      <td>{{dataItem.mainStateNum}}</td>
      <td>{{dataItem.taxNum}}</td>
      <td>{{dataItem.regDate.toDateString()}}</td>
      <td class="edit-area">
        <button class="edit-button" @click="editCell(dataItem.id)">E</button>
        <button class="delete-button" v-on:click="removeCompany(dataItem.id)">&times;</button>
      </td>
    </tr>
  </tbody> 
</template> 

<script>
export default {
  props: ['dataList'],
  components: {
  },
  data () {
    return {
      inputData : ''
    }
  },
  methods: {
    removeCompany(companyId) {
      this.$emit('remove-company', companyId);
    },

    editCell(id) {
      this.dataList.forEach(e => e.isEditing = false);
      this.dataList.find(e => e.id === id).isEditing = true;
    },

    saveCell(id) {
      this.dataList.find(e => e.id === id).name = this.inputData;
      this.dataList.find(e => e.id === id).isEditing = false;
    }
  }
};
</script>

<style>
  td button {
    background-color: rgb(93, 180, 93);
    margin: 0;
    padding: 4px 0;
    width: 30px;
    height: 30px;
    font-weight: bold;
    border: 3px dashed black;
    border-radius: 50%;
  }

  .edit-area {
    display: flex;
    justify-content: space-between; 
    margin: 0;
    border-collapse: collapse;
  }

  td .delete-button {
    margin-left: 10px;
    background-color: rgb(189, 96, 96);
  }

  td button:hover {
    border: 3px solid black;
    background-color: white;
  }
</style>