<script>
    import Home from './Home.svelte';
    import Learn from './Learn.svelte';
    import MultipleChoice from './MultipleChoice.svelte';
    import Help from './Help.svelte';
    import Flashcard from './Flashcard.svelte';
    //'home' = home, 'mc' = multiple choice, 'terms' = terms, ...
    var screenNum = 'home';
    let globalmap = new Map();
    let dataCache = '';

    const whitelist = ['help', 'home'];

    function handleScreenChange(input) {
        if (globalmap.size == 0){
            if (whitelist.includes(input)){
                screenNum = input;
            }
            return;
        }

        screenNum = input;
    }
</script> 

<style>
    .title {
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: xx-large;
        margin: 0%;
        padding: 1vh;
        font-weight: bold;
        display: inline-block;
        color: white;
    }

    .menu-button {
        border: 1px;
        color: white;
        text-align: center;
        vertical-align: middle;
        display: inline;
        background-color: transparent;
        font-size: large;
        margin: 0;
        margin-left: 10px;
        border-radius: 8px;
    }

    .menu-button:hover {
        background-color: #087742;
    }

    .banner {
        background-color: #054A29;
        width: 100%;
        margin: 0;
        padding: 0;
        border: 0;
        display: inline-flex;
    }
</style>

<title>Quiz Me</title>

<div class="banner">
    <h1 class="title">Quiz Me |</h1>
    <button class="menu-button" on:click={() => handleScreenChange('home')}>Home</button>
    <button class="menu-button" on:click={() => handleScreenChange('flash')}>Flashcard</button>
    <button class="menu-button" on:click={() => handleScreenChange('mc')}>Multiple Choice</button>
    <button class="menu-button" on:click={() => handleScreenChange('learn')}>Learn</button>
    <button class="menu-button" on:click={() => handleScreenChange('help')}>Help</button>
</div>

{#if screenNum == 'home'}
    <Home bind:map={globalmap} bind:data={dataCache}/>
{:else if screenNum == 'mc'}
    <MultipleChoice map={globalmap}/>
{:else if screenNum == 'learn'}
    <Learn map={globalmap}/>
{:else if screenNum == 'help'}
    <Help/>
{:else if screenNum == 'flash'}
    <Flashcard map={globalmap}/>    
{/if}