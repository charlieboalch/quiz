<style>
    :global(body) {
        margin: 0px;
        padding: 0px;
    }

    #quiz-data-div {
        width: 48%;
        float: left;
        height: 87vh;
        margin-top: 0.5em;
        margin-left: 0.5em;
    }

    #map-display-div {
        width: 50%;
        float: right;
        height: 87vh;
        overflow: scroll;
        margin-top: 0.5em;
        margin-right: 0.5em;
    }

    textarea {
        width: 95%;
        height: 100%;
        resize: none;
    }

    p {
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: medium;
    }

    hr {
        border-top: 1px solid lightgray;
    }
</style>

<script>
    //import fetch from "node-fetch";
    import { createEventDispatcher, onMount } from 'svelte';

    const dispatch = createEventDispatcher();

    export let data = '';
    export let map = new Map();
    let mapTermDiv;
    let newTerm = '';
    let newValue = '';
    let mapTerms = [];

    function parseData () {
        if (data == '') return;
        mapTerms = [];
        map = new Map();
        const datalist = data.split(";;");
        datalist.splice(datalist.length - 1, 1);
        for (let i = 0; i < datalist.length; i++){
            try{
                const list = datalist[i].split("?:");
                map.set(list[0], list[1]);
            } catch (err){
                alert("Error parsing data! Is it formatted correctly?");
                continue;
            }
        }
        map = map;
        mapTerms = Array.from(map.keys());
        dataCache = data;
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

    onMount(() => {
        mapTerms = Array.from(map.keys());
        data = data;
    });

    function resetData() {
        data = '';
        mapTerms = [];
        map = new Map();
    }

    function addTerm() {
        map.set(newTerm, newValue);
        map = map;
        mapTerms = Array.from(map.keys());
        data = data + newTerm + "?:" + newValue + ";;";
        newTerm = '';
        newValue = '';
        scrollToBottom(mapTermDiv);
    }

    function scrollToBottom (node) {
        node.scroll({ top: node.scrollHeight, behavior: 'smooth' });
    };
</script>

<div id="quiz-data-div">
    <textarea placeholder="Data" bind:value={data}></textarea>
    <button on:click={parseData}>Load</button>
    <button on:click={resetData}>Reset</button>
</div>

<div id="map-display-div" bind:this={mapTermDiv}>
    {#each mapTerms as term}
        <p>{term}: {map.get(term)}</p>
        <hr>
    {/each}

    <div>
        <input bind:value={newTerm} placeholder="New term" style="display: inline-block;">
        <input bind:value={newValue} placeholder="New value" style="display: inline-block;">
        <button on:click={addTerm}>Add term</button>
    </div>
</div>