<script>
    import { API_URL } from "$lib";
    import { get } from "svelte/store";
    import { Goggles, Pencil, Trash } from "phosphor-svelte"

    import axios from "axios";
    import Loader from "$lib/components/Loader.svelte";
    import Modal from "$lib/components/Modal.svelte";
    import { goto } from "$app/navigation";

    let loader_toggle = $state(false)
    let products = $state()
    let modal_data = $state();
    function showModal(message, cancel, redirect){
        modal_data = {
            "toggle": true,
            "message": message,
            "cancel": cancel,
            "redirect": redirect,
        }
    }

    async function loadProducts() {
        axios.get(API_URL + '/api/products/')
        .then(function (response) {
            products = response.data.data
        })
        .catch(function (error) {
            console.log(error);
        })
    }

    loadProducts()

    async function deleteProduct(productId) {
        const shouldDelete = window.confirm("Are you sure to delete?");
        if (!shouldDelete)
        {
            return
        }

        loader_toggle = true

        let options = {
            method: "DELETE",
            headers: {
                "Content-Type": "application/json",
            },
            withCredentials: true,
        };

        axios(API_URL + '/api/products/' + productId, options)
        .then(function (response) {
            if(response.status === 204)
            {
                showModal("Deleted product successfully...!", "Continue")
                loadProducts()
            }
        })
        .catch(function (error) {
            showModal(error.response.data.detail, "Try Again")
        })
        .finally(function () {
            loader_toggle = false
        })
    }

    async function editProduct(productId){
        goto('/admin/edit-product/' + productId)
    }

</script>

{#if loader_toggle}
<div class="delete-loader">
    <Loader/>
</div>
{/if}
{#if products}
    <table>
        <thead >
            <tr>
                <th class="name">Name</th>
                <th class="icon">Icon</th>
                <th class="price">Price</th>
                <th class="action">Actions</th>
            </tr>
        </thead>
        <tbody>
            {#each products as product}
            <tr> 
                <td class="name">{product['name']}</td>
                <td class="icon">
                    <img src={product['icon_url']} alt="">
                </td>
                <td class="price ">Rs. {product['price']}</td>
                <td class="action text-white">
                    <button class="bg-blue-300 px-6 py-2 rounded-xs m-1" onclick={ () => { editProduct(product['$id']) }} >
                        <Pencil/>
                    </button>
                    <button class="bg-red-600 px-6 py-2 rounded-xs m-1" onclick={ () => { deleteProduct(product['$id'])}} >
                        <Trash/>
                    </button>
                </td>
            </tr>
            {/each}
        </tbody>
    </table>
{:else}
    <Loader/>
{/if}

<Modal {...modal_data}/>

<style>
    table{
        position: relative;
        width: 100%;
        padding: 1rem;
        border: 1px solid hsl(0, 1%, 85%);    
    }

    th, td{
        padding: 1rem;
    }

    th{
        color: white;
        background-color: hsl(0, 1%, 22%);
        text-align: left;
    }

    tr:nth-last-of-type(2n) {
        background-color: hsl(0, 0%, 95%);
    }

    .icon{
        text-align: center;
        img {
            margin: auto;
            max-width: 75px;
        }
    }

    .action{
        text-align: center;

        button:active{
            transform: scale(.95);
        }
    }


    @media (max-width: 650px){
        th{
            display: none;
        }

        td {
            display: block;
        }

        td:first-child {
            padding-top: 2rem;
        }

        td:last-child {
            padding-bottom: 2rem;
        }

    }

    .delete-loader{
        position: absolute;
        right: calc(50% - 25px - 5rem);
        top: 42%;
        z-index: 99;
        border-radius: .3rem;
        border: 1px solid rgb(209, 209, 209);
        background-color: #fff;
        padding-inline: 5rem;
        box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
    }
</style>