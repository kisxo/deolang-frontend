<script>
    import { API_URL } from "$lib";
    import { get } from "svelte/store";
    import { Pencil, Trash } from "phosphor-svelte"

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
            <td class="price">Price</td>
            <td class="action text-white">
                <button class="bg-blue-300 px-4 py-1 rounded-xs m-1">
                    <Pencil/>
                </button>
                <button class="bg-red-600 px-4 py-1 rounded-xs m-1">
                    <Trash/>
                </button>
            </td>
		</tr>
        {/each}
	</tbody>
</table>

<style>
    table{
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

</style>