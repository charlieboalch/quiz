<style>
    div {
        width: 100%;
        text-align: center;
    }

    span {
        display: inline-block;
        text-align: center;
        background-color: aliceblue;
        font-size: large;
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        margin: 0.2em;
        margin-bottom: 1em;
    }
</style>

<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let map;
    if (!(map instanceof Map)) throw new TypeError("map is not instace of Map");

    const keys = Array.from(map.entries());
    console.log(keys.toString())

    function returnHome(){
        dispatch("message", {
            text: 'home'
        });
    }
</script>

{#each keys as key}
    <div>
        <span>{key[0]}</span>
        <span>{key[1]}</span>
    </div>
{/each}

<div>
    <button on:click={returnHome}>Home</button>
</div>