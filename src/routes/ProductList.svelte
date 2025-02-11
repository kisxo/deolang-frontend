<script>
    import ProductCard from "./ProductCard.svelte";

    import { API_URL } from "$lib";
    import { get } from "svelte/store";

    import axios from "axios";

    let products = $state()

    axios.get(API_URL + '/products/')
    .then(function (response) {
        products = response.data.data
    })
    .catch(function (error) {
        console.log(error);
    })   
</script>

<div class="list">
    {#each products as product}
        <ProductCard {...product}></ProductCard>
    {/each}
</div>

<style>
    .list{
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        gap: 1.5rem;
        padding: 2rem;
    }
</style>

