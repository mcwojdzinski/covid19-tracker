<template>
    <table>
        <tr v-for="country in countries" :key="country">
            <td> {{ country.country }}  </td>
            <td><strong> {{ formatDigits(country.cases) }} </strong></td>
        </tr>
    </table>
</template>
<script>
export default {
    name: "Table",
    props: {
        countries: String,
    },
    methods:{
        formatDigits: function(number){
            return new Intl.NumberFormat('en-US').format(number);
        }
    }
}
</script>
<style lang="scss">
table{
    display: block;
    width: 100%;
    position: relative;
    height: 500px;
    overflow-y: scroll;
    color: #6a5d5d;
    border-spacing: 0;

    tr:nth-of-type(odd) {
        background-color: #f3f2f8;
    }

    tr{
        display: flex;
        justify-content: space-between;
    }

    td {
        padding: 10px;
    }
}
</style>