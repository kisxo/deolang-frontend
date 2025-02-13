<script>
    import { API_URL } from "$lib";
    import axios from "axios";

    import Loader from "$lib/components/Loader.svelte";
    import ProfileCard from "./ProfileCard.svelte";
    import { goto } from "$app/navigation";

    let user = $state()

    axios.get(API_URL + '/api/auth/user', {withCredentials: true})
    .then((response) => {
        if(response.status === 200)
        {
            user = response.data.data
        }
    })
    .catch(function (error) {
        
    })
    .finally(function () {
        if(!user)
        {
            goto('/auth')
        }
    });
</script>

<div class="text-4xl py-4 font-bold ps-4 border-b">Profile</div>

{#if user}
    <ProfileCard {...user}/>
{:else}
    <Loader/>
{/if}
