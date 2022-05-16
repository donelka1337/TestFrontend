<template>
    <div class ="loading"></div>
</template>

<script>
export default {

}
</script>

<style>
    .loading {
        align-items: center;
        display: flex;
        justify-content: center;
    }

    .loading::after{
        content: "";
        width: 50px;
        height: 50px;
        border: 10px solid #dddddd;
        border-top-color: black;
        border-radius: 50%;
        animation: loading 1s linear infinite;
    }

    @keyframes loading {
        to {
            transform: rotate(1turn);
        }
    }
</style>