<script>
    import Home from './Home.svelte';
    import MultipleChoice from './MultipleChoice.svelte';
    import Terms from './Terms.svelte';
    //'home' = home, 'mc' = multiple choice, 'terms' = terms, ...
    var screenNum = 'home';
    let globalmap = new Map();

    function handleScreenChange(event) {
        screenNum = event.detail.text;
    }
</script> 

<title>Quiz Me</title>

{#if screenNum == 'home'}
    <Home on:message={handleScreenChange} bind:map={globalmap}/>
{:else if screenNum == 'mc'}
    <MultipleChoice map={globalmap} on:message={handleScreenChange}/>
{:else if screenNum == 'terms'}
    <Terms map={globalmap} on:message={handleScreenChange}/>
{/if}