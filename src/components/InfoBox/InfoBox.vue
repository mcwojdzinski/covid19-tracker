<template>
    <div class="card">
        <h1>{{ title }}</h1>
        <p> + {{cases}}</p>
        <p>{{total}} Total</p>
    </div>
</template>
<script>
export default {
    name: "InfoBox",
    props: {
        title: String,
        total: Number,
        cases: Number
    }
}
</script>
<style lang="scss">
    .card{
        width: 33.333%;
        height: 20vh;
        background-color: white;
        padding: 1rem;
        box-shadow: 0 0 8px -4px rgba(0,0,0,0.5);
        margin: 5px;
        padding: 10px;
        border-radius: 10px;
    }
</style>