<svg class="container">
    <rect class="boxes"></rect>
</svg>

<style>
    .container {
        width: 50px;
        height: 50px;
        margin-inline: auto;
        margin-block: 3rem;
    }

    .container .boxes {
        width: 50px;
        height: 50px;
        fill: none;
        stroke-width: 50px;
        stroke: var(--primary-600);
        stroke-dasharray: 50;
        stroke-dashoffset: 50%;
        animation: animate 2s linear infinite;
    }

    @keyframes animate {
        to {
            stroke-dashoffset: 250%;
        }
    }

</style>