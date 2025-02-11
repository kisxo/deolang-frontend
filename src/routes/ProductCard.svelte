<script>
    let {name, description, price, icon, category} = $props()
    function commingSoon(){
        alert("Comming Soon....!")
    }
</script>

<div class="card">
    <img src={icon} alt="">
    <span class="price">&#8377; {price}</span>
    <span class="name">{name}</span>
    <a class="buy-button" onclick={commingSoon}  href="/" aria-label="buy button">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 256 256">
            <rect width="256" height="256" fill="none"/>
            <path d="M188,184H91.17a16,16,0,0,1-15.74-13.14L48.73,24H24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="20"/>
            <circle cx="92" cy="204" r="20" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="20"/>
            <circle cx="188" cy="204" r="20" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="20"/>
            <path d="M70.55,144H196.1a16,16,0,0,0,15.74-13.14L224,64H56" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="20"/>
        </svg>
    </a>
</div>

<style>
    .card{
        font-size: .9rem;
        width: 400px;
        border: 1px solid var(--primary-100);
        display: flex;
        flex-direction: column;
        padding: 1rem;
        gap: .8rem;
        box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;

        img{
            max-height: 200px;
            width: 400px;
            object-fit: contain;
            margin-bottom: .5rem;
        }
    }

    .price{
        font-size: 1.1rem;
    }

    .buy-button{
        margin-top: auto;
        font-weight: bold;
        font-size: .8rem;
        border-radius: .15rem;
        padding-block: .3rem;
        background-color: var(--primary-500);

        svg{
            color: #fff;
            margin-inline: auto;
            height: 20px;
            width: 20px;
        }
    }

    .buy-button:active{
        transform: scale(.98);
    }
</style>