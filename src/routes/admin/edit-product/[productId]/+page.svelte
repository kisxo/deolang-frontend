<script>
    import { page } from "$app/state";
    import axios from "axios";
    import { API_URL } from "$lib";

    import Loader from "$lib/components/Loader.svelte";
    import EditForm from "../EditForm.svelte";

    let product = $state()

    async function loadProduct() {
        axios.get(API_URL + '/api/products/' + page.params.productId)
        .then(function (response) {
            product = response.data.data
        })
        .catch(function (error) {

            console.log(error);
        })
    }

    loadProduct()
</script>


{#if product}
    <EditForm {...product}/>
{:else}
    <Loader/>
{/if}