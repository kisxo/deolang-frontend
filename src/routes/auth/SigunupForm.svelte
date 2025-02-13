<script>
    import axios from "axios";
    import { API_URL } from "$lib";

    import Modal from "$lib/components/Modal.svelte";
    import Loader from "$lib/components/Loader.svelte";

    let {email, password, cpassword} = $state('')
    let loader_toggle = $state(false)
    let modal_data = $state();
    function showModal(message, cancel, redirect, callback){
        modal_data = {
            "toggle": true,
            "message": message,
            "cancel": cancel,
            "redirect": redirect,
            "callback": callback,
        }
    }

    async function signup() {
        if(password != cpassword)
        {
            showModal("Password Does Not Match !")
            return
        }

        if(password.length < 8)
        {
            
            showModal("Password length must be atleast 8 characters !")
            return
        }

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

        loader_toggle = true
        axios.post(API_URL + '/api/users/', data, options)
        .then((response) => {
            loader_toggle = false
            if (response.status ===201)
            {   
                showModal("Signup Successful", "Login", "/profile")
            }
        })
        .catch(function (error) {
            loader_toggle = false
            
            if(error.response.status === 422)
            {   
                showModal("Invalid input data !")
            }
        });
    }
</script>

<div class="signup-form">
    {#if loader_toggle}
        <Loader/>
    {:else}
        <label for="id"> Email </label>
        <input type="email" id="email" bind:value={email}>

        <label for="password">Password</label>
        <input type="password" id="password" bind:value={password}>

        <label for="cpassword">Confirm Password</label>
        <input type="password" id="cpassword" bind:value={cpassword}>

        <button onclick={signup}>Register</button>
    {/if}



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