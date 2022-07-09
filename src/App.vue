<template>
  <body id="app">
    <header>
      <div class="header-label"></div>
      <div class="header-title">
        <div class="add-section">
          <button @click="showAddComponent" class="add-button">+
          </button>
          <p class="button-desc">Добавить компанию</p>
        </div>
        <h1>Список компаний</h1>  
      </div>
    </header>
    <main>
      <div class="table-container">
        <form id="addCompanyForm" @submit.prevent></form>
      <table class="resp-tab">
        <thead>
          <tr>
            <th>Наименование</th>
            <th>Адрес</th>
            <th>ОГРН</th>
            <th>ИНН</th>
            <th>Дата регистрации</th>
            <th class="button-cell-title"> </th>
          </tr>
        </thead>
          <AddForm
            v-bind:isAddActive="isAddActive"
            @getInp="onSubmit"
            @closeForm="showAddComponent"
          />
          <CompaniesList
            v-bind:dataList="dataList"
            @remove-company="removeCompany"
          />
        
      </table>
      </div>
    </main>
    <footer></footer>
  </body>
</template>

<script>
var url = "https://cleaner.dadata.ru/api/v1/clean/address";
var token = "3b290bc3b2dc08897192712a56883668306834a8";
var secret = "cee6788c34e2551acd31eeb749ef60adc2f10649";
var query = "мск сухонска 11/-89";

var options = {
    method: "POST",
    mode: "cors",
    headers: {
        "Content-Type": "application/json",
        "Authorization": "Token " + token,
        "X-Secret": secret
    },
    body: JSON.stringify([query])
}

fetch(url, options)
.then(response => response.text())
.then(result => console.log(result))
.catch(error => console.log("error", error));

import CompaniesList from "./CompaniesList.vue";
import AddForm from "./AddForm.vue";
export default {
  name: "App",
  data() {
    return {
      dataList : [
        {id: 1, address : "Mulberry Street 32", name: "Burger King", mainStateNum: '6789087698312', taxNum: '763348719', regDate: new Date('1960-05-01'), isEditing : false},
        {id: 2, address : "Komsomolskaya 98", name: "State Food", mainStateNum: '8786281694357', taxNum: '163245823', regDate: new Date('2006-01-21'), isEditing : false},
        {id: 3, address : "Liberty Prospect 15", name: "Onyx INC", mainStateNum: '4286182295364', taxNum: '653149289', regDate: new Date('1995-06-25'), isEditing : false}
      ],
      form : {

      },
      isAddActive : false
    };
  },
  methods: {
    removeCompany(companyId) {
      this.dataList = this.dataList.filter(data => data.id !== companyId);
    },
    showAddComponent() {
      this.isAddActive = !this.isAddActive
    },

    onSubmit(update) {
      // /[0-9]{12}/.test(update.taxNum) ? console.log("норм") : console.log("хуйня");
      //if (/[0-9]{12}/.test(update.taxNum) && /[0-9]{13}/.test(update.mainStateNum)) {
      //  console.log("успех");
      //}
      for (var e in update) {
        if (update[e] === null || update[e] === "") {
          return;
        }
      }
      const newCompany = {
        id: new Date(),
        address: update.address.trim(),
        name: update.name.trim(),
        mainStateNum: update.mainStateNum.trim(),
        taxNum: update.taxNum.trim(),
        regDate: new Date(update.regDate),
        isEditing: false
      }
      
      this.dataList.push(newCompany);
    }
  },
  components: {
    CompaniesList,
    AddForm
  },
};
</script>

<style>
#app {
  margin: 0;
  padding: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background: linear-gradient(90deg, #e7edf8, #C5DDE8);
}

header {
  background-color: #112333;
  height: 100px;
}

.header-title {
  width: 830px;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: auto;
  font-size: 40px;
}

.header-title h1{
  font-size: 40px;
  color: #dddddd;
  padding-bottom: 5px;
  border-bottom: 4px solid #dddddd;
}

.add-section {
  height: 90%;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.add-button {
  color: #112333;
  width: 150px;
  background-color: rgb(93, 180, 93);
  border: 3px solid rgb(255, 255, 255);
  height: 50px;
  font-size: 50px;
  line-height: 20px;
}

.add-button:hover {
  background-color: rgb(255, 255, 255);
  border: 0;
}

.button-desc {
  margin: 0;
  font-size: 10px;
  padding: 4px 0;
  color: #dddddd;
  font-weight: bold;
  border: 3px dashed #dddddd;
  text-transform: uppercase;  
}

.table-container {
  width: 1100px;
  margin: 20px auto 0 auto;
}

.resp-tab {
    margin-bottom: 100px;
    background: #ffffff;
    border-radius: 5px;
    font-weight: normal;
    border: none;
    border-collapse: collapse;
    width: 100%;
}
.resp-tab th, .resp-tab td {
    padding: 10px 20px;
    font-size: 13px;
    border: none;
    border: 1px solid #222222;
    border-collapse: collapse;
    vertical-align: top;
}

.button-cell-title {
  width: 40px;
}

.resp-tab th {
    color: #FFF;
    background: #112333;
    font-weight: bold;
    border: 1px solid #dddddd;    
    text-transform: uppercase;    
    text-align: center;    
}
.resp-tab tr:nth-child(even) {
    background: #dddddd;
}


@media(max-width: 100px) {
    .resp-tab thead {
        display: none;
    }
    .resp-tab tr {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        margin-bottom: 30px;
    }
    .resp-tab td {
        margin: 0 -1px -1px 0;
        padding-top: 35px;
        width: 50%;
        border: none;
    }
    .resp-tab td span {
        display: block;
    }
}

</style>