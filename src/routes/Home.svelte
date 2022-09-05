<style>
    .title {
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: 50px;
        padding-top: 5%;
    }

    .centered {
        width: 100%;
        text-align: center;
    }

    .flavortext {
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: large;
    }

    .input {
        margin: 5%;
        margin-right: 0%;
        margin-left: 35%;
        margin-bottom: 0%;
        max-width: 25%;
        text-align: left;
    }

    .button {
        margin-top: 5px;
    }
</style>

<script>
    //import fetch from "node-fetch";
    import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

    let data = '';
    let isLoaded = false;
    export let map = new Map();

    function parseData () {
        isLoaded = false;
        const datalist = data.split(";;");
        for (let i = 0; i < datalist.length; i++){
            const list = datalist[i].split("?:");
            map.set(list[0], list[1]);
        }
        data = '';
        isLoaded = true;
        map = map;
    }

    function broadcastMultiple(){
        dispatch('message', {
            text: 'mc'
        });
    }
</script>

<div class="title centered">
    Quiz Me
</div>

<div class="flavortext centered">
    now portable
</div>

<div>
    <input class="input centered" type="text" placeholder="Quizlet data" bind:value="{data}">
    <button on:click="{parseData}">Load</button>
</div>

{#if map.size != 0}
    <div class="centered">
        <button class="button" on:click={broadcastMultiple}>Multiple Choice</button>
        <button class="button">Learn</button>
        <button class="button">Terms</button>
    </div>
{/if}