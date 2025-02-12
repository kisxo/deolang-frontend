<script>
    import axios from "axios";
    import { API_URL } from "$lib";
    import { goto } from "$app/navigation";

    import Loader from "$lib/components/Loader.svelte";
    import AdminDashboard from "./AdminDashboard.svelte";

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
        <AdminDashboard/>
    {:else}
        <h2>Not a Admin</h2>
    {/if}
{/if}