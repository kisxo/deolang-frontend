<script>
    import { API_URL } from "$lib";
    import { get } from "svelte/store";
    import axios from "axios";

    import ProductCard from "./ProductCard.svelte";
    import Loader from "$lib/components/Loader.svelte";

    let products = $state()

    axios.get(API_URL + '/api/products/')
    .then(function (response) {
        products = response.data.data
    })
    .catch(function (error) {
        console.log(error);
    })   
</script>

{#if products}
    <div class="list">
        {#each products as product}
            <ProductCard {...product}></ProductCard>
        {/each}
    </div>
{:else}
    <Loader/>
{/if}

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

