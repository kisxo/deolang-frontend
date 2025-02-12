<script>
    import { goto } from "$app/navigation";
    import { API_URL } from "$lib";
    import axios from "axios";
    import Cookies from "js-cookie";

    import Modal from "$lib/components/Modal.svelte";

    let email = $state('');
    let password = $state('');
    let modal_data = $state();
    function showModal(message, cancel, redirect){
        modal_data = {
            "toggle": true,
            "message": message,
            "cancel": cancel,
            "redirect": redirect,
        }
    }

    async function signin() {
        let data = {
            "email": email,
            "password": password,
        };
        let options = {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            withCredentials: true,
        };

        axios.post(API_URL + '/api/auth/token', data, options)
        .then((response) => {
            if (response.status ===200)
            {   
                showModal(response.data.message, "Continue", "/profile")
            }
        })
        .catch(function (error) {
            showModal(error.response.data.detail)
        });
    }
</script>

<div class="signup-form">

    <label for="id"> Email </label>
    <input type="email" id="email" bind:value={email}>

    <label for="password">Password</label>
    <input type="password" id="password" bind:value={password}>

    <button onclick={signin}>Login</button>

</div>

<Modal {...modal_data}/>

<style>
    .signup-form{
        max-width: 700px;
        padding: 1rem;
        /* margin-inline: auto; */
        display: flex;
        flex-direction: column;
        gap: 1rem;

        input{
            padding: .5rem;
            outline: 1px solid #ddd;
        }

        button{
            margin-top: 1rem;
            color: #ffffff;
            font-weight: bold;
            font-size: .8rem;
            border-radius: .15rem;
            padding-block: .5rem;
            background-color: var(--primary-500);
        }
        button:active{
         transform: scale(.98);
        }
    }
</style>