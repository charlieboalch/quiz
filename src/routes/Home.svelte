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
        display: inline;
        margin-right: 0.5em;
    }

    hr {
        border-top: 1px solid lightgray;
    }

    .remove-term{
        border: 0;
        background-color: #fff6f6f8;
        color: gray;
        font-weight: bolder;
        border-radius: 8px;
    }
</style>

<script>
    //import fetch from "node-fetch";
    import { onMount } from 'svelte';

    export let data = '';
    export let map = new Map();
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
        data = generateMapString();
        mapTerms = Array.from(map.keys());
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
    }

    function removeTerm(term) {
        map.delete(term);
        data = generateMapString();
        map = map;
        mapTerms = Array.from(map.keys());
    }

    function generateMapString() {
        let base = '';
        for (const key of map.keys()){
            base = base + key + "?:" + map.get(key) + ";;";
        }
        return base; 
    }
</script>

<div id="quiz-data-div">
    <textarea placeholder="Data" bind:value={data}></textarea>
    <button on:click={parseData}>Load</button>
    <button on:click={resetData}>Reset</button>
    <button on:click={swapKeyValue}>Swap Terms</button>
</div>

<div id="map-display-div">
    {#each mapTerms as term}
        <div>
            <p>{term}: {map.get(term)}</p>
            <button class="remove-term" on:click={() => removeTerm(term)}>X</button>
            <hr>
        </div>
    {/each}

    <div>
        <input bind:value={newTerm} placeholder="New term" style="display: inline-block;">
        <input bind:value={newValue} placeholder="New value" style="display: inline-block;">
        <button on:click={addTerm}>Add term</button>
    </div>
</div>