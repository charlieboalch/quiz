<script>
    import Home from './Home.svelte';
    import MultipleChoice from './MultipleChoice.svelte';
    //'home' = home, 'mc' = multiple choice...
    var screenNum = 'home';
    let globalmap = new Map();

    function handleScreenChange(event) {
        screenNum = event.detail.text;
    }
</script>

{#if screenNum == 'home'}
    <Home on:message={handleScreenChange} bind:map={globalmap}/>
{:else if screenNum == 'mc'}
    <MultipleChoice map={globalmap} on:message={handleScreenChange}/>
{/if}