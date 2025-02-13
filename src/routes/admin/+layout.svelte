<script>
    let { children } = $props();
    import { PlusCircle } from "phosphor-svelte"
    import axios from "axios";
    import { API_URL } from "$lib";
    import { goto } from "$app/navigation";
    import { page } from "$app/state";

    import Loader from "$lib/components/Loader.svelte";
    import AdminProductList from "./dashboard/AdminProductList.svelte";

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
        <div class="admin-dashboard">
            <div class="title text-slate-800 ">
                <a href="/admin/dashboard" class="me-auto flex items-center">Admin Dashboard</a>
                {#if page.url.pathname === '/admin/dashboard'}
                    <a href="/admin/add-product" class="flex items-center border-1 p-[.2rem] border-slate-400 rounded">
                        <PlusCircle/>Add Product
                    </a>
                {/if}
            </div>
            {@render children?.()}
        </div>
    {:else}
        <h2>Not a Admin</h2>
    {/if}
{/if}

<style>
    .admin-dashboard{
        margin: 1rem;
        display: flex;
        gap: 1rem;
        flex-direction: column;
    }

    .title{
        display: flex;
        padding-block: 1rem;
        font-size: 1.4rem;
        font-weight: 600;
        border-bottom: 1px solid grey;
    }
</style>