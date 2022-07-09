<template>
    <tbody v-if="isAddActive">
        <tr>
            <td><input type="text" form="addCompanyForm" v-model="name" required></td>
            <td><input type="text" form="addCompanyForm" v-model="address" required></td>
            <td><input type="number" form="addCompanyForm" v-model="mainStateNum" required></td>
            <td><input type="number" form="addCompanyForm" v-model="taxNum" required></td>
            <td><input type="date" form="addCompanyForm" v-model="regDate"></td>
            <td class="edit-area">
                <button type="submit" class="submit-button" @click='getInp()' form="addCompanyForm">&#10004;</button>
                <button class="delete-button" form="addCompanyForm" @click="closeForm($event)">&times;</button>
            </td>
        </tr>
    </tbody>
</template>

<script>
    export default {
        props: ['isAddActive'],
        methods: {
            getInp () {
                this.$emit('getInp', {
                    name: this.name,
                    address: this.address,
                    mainStateNum: this.mainStateNum,
                    taxNum: this.taxNum,
                    regDate: this.regDate
                });
                this.$emit('closeForm');
                this.name = "";
                this.address = "";
                this.mainStateNum = "";
                this.taxNum = "";
                this.regDate = "";
            },
            closeForm () {
                this.name = "";
                this.address = "";
                this.mainStateNum = "";
                this.taxNum = "";
                this.regDate = "";
                this.$emit('closeForm');
            }
        },
        data () {
        return {
            name: '',
            address: '',
            mainStateNum: '',
            taxNum: '',
            regDate: new Date()
        }
    }
    }
</script> 

<style scoped>
    tr {
        border: 5px solid rgb(14, 117, 14);
    }

    tr td {
        background-color: rgb(169, 224, 169);
    }

    input {
        border: 0;
        height: 20px;
        margin: 0 0 7px 0;
    }

    .button {
        border: 0;
        height: 20px;
    }
</style>