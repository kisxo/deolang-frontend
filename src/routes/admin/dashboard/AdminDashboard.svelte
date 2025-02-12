<script>
    import AdminProductList from "./AdminProductList.svelte";
</script>

<div class="admin-dashboard">
    <span class="title">
        Admin Dashboard
    </span>
    <AdminProductList/>
</div>

<style>
    .admin-dashboard{
        margin: 1rem;
        display: flex;
        gap: 1rem;
        flex-direction: column;
    }

    .title{
        padding-block: 1rem;
        font-size: 1.5rem;
        border-bottom: 1px solid grey;
    }
</style>