<script>
    let user = $props()


</script>

<div class="profile-card">
    <span class="title">Email</span>
    <span class="data">{user.email}</span>

    <span class="title">Creation Date</span>
    <span class="data">{user.registration}</span>

    <span class="title">User Type</span>
    {#if user.labels.includes("admin")}
        <span class="data">Admin</span>
    {:else}
        <span class="data">User</span>
    {/if}
</div>

<style>
    .profile-card{
        margin-block: 4rem;
        margin-inline: auto;
        max-width: 400px;
        padding: 3rem;
        outline: 1px solid #ddd;
        display: flex;
        flex-direction: column;
        box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
    }

    .title{
        font-weight: 800;
    }

    .data{
        padding-bottom: 2rem;
    }
</style>