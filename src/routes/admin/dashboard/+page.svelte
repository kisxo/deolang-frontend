<script>
    import axios from "axios";
    import { API_URL } from "$lib";

    import Loader from "$lib/components/Loader.svelte";
    import { goto } from "$app/navigation";

    let loader_toggle = $state(true)
    let is_admin = $state(false)

    let options = {
        method: "GET",
        headers: {
            "Content-Type": "application/json",
        },
        withCredentials: true,
    };

    loader_toggle = true
    axios(API_URL + '/api/auth/user', options)
    .then((response) => {
        loader_toggle = false
        if (response.data.data.labels.includes("admin") )
        {
            is_admin = true
        }
    })
    .catch(function (error) {
        loader_toggle = false
    });

</script>

{#if loader_toggle}
    <Loader/>
{:else}
    {#if is_admin}
        <h2>Is Admin</h2>
    {:else}
        <h2>Not a Admin</h2>
    {/if}
{/if}


