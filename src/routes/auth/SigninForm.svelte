<script>
    import { APPWRITE_API_ENDPOINT, APPWRITE_PROJECT_ID} from "$lib";
    import { Client, Account, ID } from "appwrite";

    import Modal from "$lib/components/Modal.svelte";

    const client = new Client()
    .setEndpoint(APPWRITE_API_ENDPOINT)
    .setProject(APPWRITE_PROJECT_ID);

    const account = new Account(client);

    let {email, password} = $state('')
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
       
        const promise = account.createEmailPasswordSession(email, password);

        promise.then(function (response) {
            showModal("Signin Successfull", "Continue", "/")
        }, function (error) {
            showModal(error.message)
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