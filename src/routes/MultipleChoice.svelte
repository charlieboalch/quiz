<script>
    import { createEventDispatcher, onMount } from 'svelte';
    export let map;
    let prompt = '';

    const dispatch = createEventDispatcher();

    //init logic
    onMount(() => {
        if (!(map instanceof Map)){
            throw new TypeError("Map is incorrect type, contact server administrator");
        }

        if (map.size < 4){
            alert("There are not enough terms in this set for this mode, sorry!");
            dispatch('message', {
                text: 'home'
            });
        }

        
    });

    function onButtonClick(arg){
        alert(arg);
    }
</script>

<style>
    .centered {
        width: 100%;
        text-align: center
    }

    .title {
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: 50px;
        padding-top: 5%;
        font-weight: 300;
    }

    .option {
        width: 15%;
        font-size: large;
    }
</style>

<h1 class="centered title">filler</h1>

<div class="centered">
    <button class="option" on:click={() => onButtonClick('button1')}>e</button>
    <button class="option" on:click={() => onButtonClick('button1')}>f</button>
    <button class="option" on:click={() => onButtonClick('button1')}>g</button>
    <button class="option" on:click={() => onButtonClick('button1')}>h</button>
</div>