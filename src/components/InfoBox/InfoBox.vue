<template>
    <div :class="['card', selectedTypeAndActive(active, type)]">
        <h1 class="card--header">{{ title }}</h1>
        <p :class="['card--cases', changeColor(active, type)]"> + {{cases}}</p>
        <p class="card--total">{{total}} Total</p>
    </div>
</template>
<script>
export default {
    name: "InfoBox",
    props: {
        title: String,
        total: Number,
        cases: Number,
        active: Boolean,
        type: String
    },
    methods:{
        selectedTypeAndActive(active, type){
            if(active && type === 'recovered'){
                return 'card--green'
            }
            else if(active && (type === 'cases' || type === 'deaths')){
                return 'card--red'
            }
        },
        changeColor(active, type){
            if(active && type === 'recovered'){
                return 'card--greenText'
            }
            else if(active && (type === 'cases' || type === 'deaths')){
                return 'card--redText'
            }
        }
    }
}
</script>
<style lang="scss">
    .card{
        width: 100%;
        height: 30vh;
        background-color: white;
        box-shadow: 0 0 8px -4px rgba(0,0,0,0.5);
        margin: 5px;
        padding: 20px;
        border-radius: 10px;
        transition: 0.2s;
        cursor: pointer;
        display: flex;
        flex-direction: column;
        justify-content: space-between;

        @media (min-width: 1024px){
            width: 33%;
            height: 20vh;
        }
        
        h1{
            font-size: 1.7rem;
            @media(min-width:1024px){
                font-size: 2rem;
            }
        }

        &--cases{
            margin-top: 5px;
            font-size: 2rem;
        }

        &--total{
            font-size: 1.5rem;
        }
    }

    .card--green{
        border-bottom: 8px solid greenyellow;
    }

    .card--greenText{
        color: greenyellow;
    }

    .card--red{
        border-bottom: 8px solid red;
    }

    .card--redText{
        color: red;
    }
</style>