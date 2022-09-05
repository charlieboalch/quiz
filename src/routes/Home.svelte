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
        if (data == '') return;
        isLoaded = false;
        const datalist = data.split(";;");
        datalist.splice(datalist.length - 1, 1);
        for (let i = 0; i < datalist.length; i++){
            const list = datalist[i].split("?:");
            map.set(list[0], list[1]);
        }
        data = '';
        isLoaded = true;
        map = map;
    }

    function broadcastDestination(place){
        dispatch('message', {
            text: place
        });
    }

    function swapKeyValue(){
        if (!(map instanceof Map)) return;

        const terms = Array.from(map.keys());
        const def = Array.from(map.values());

        map.clear();
        for (let i = 0; i < terms.length; i++){
            map.set(def[i], terms[i]);
        }

        map = map;
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

<div class="centered">
    {#if map.size != 0}
        <button class="button" on:click={() => broadcastDestination('mc')}>Multiple Choice</button>
        <button class="button" on:click={() => broadcastDestination('learn')}>Learn</button>
        <button class="button" on:click={() => broadcastDestination('terms')}>Terms</button>
        <button class="button" on:click={swapKeyValue}>Swap Definitions</button>
    {/if}
    <button class="button" on:click={() => {
        alert("To use: \nGo to a Quizlet set while logged into an account, click the ... below the flashcards, select 'export', set space between term and definition to '?:' and between rows to ';;', then paste in text box.")
    }}>Help</button>
</div>