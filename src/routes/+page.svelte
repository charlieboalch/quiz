<script>
    import Home from './Home.svelte';
    import Learn from './Learn.svelte';
    import MultipleChoice from './MultipleChoice.svelte';
    //'home' = home, 'mc' = multiple choice, 'terms' = terms, ...
    var screenNum = 'home';
    let globalmap = new Map();
    let dataCache = '';

    function handleScreenChange(event) {
        screenNum = event.detail.text;
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
    <button class="menu-button" on:click={() => screenNum = 'home'}>Home</button>
    <button class="menu-button" on:click={() => screenNum = 'mc'}>Multiple Choice</button>
    <button class="menu-button" on:click={() => screenNum = 'learn'}>Learn</button>
</div>

{#if screenNum == 'home'}
    <Home on:message={handleScreenChange} bind:map={globalmap} bind:data={dataCache}/>
{:else if screenNum == 'mc'}
    <MultipleChoice map={globalmap} on:message={handleScreenChange}/>
{:else if screenNum == 'learn'}
    <Learn map={globalmap} on:message={handleScreenChange}/>
{/if}