<style>
    p {
        text-align: center;
        padding: 5em;
        margin-left: 10%;
        margin-right: 10%;
        margin-top: 10%;
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: x-large;
        border-color: #054A29;
        border-style: solid;
        border-radius: 16px;
        background-color: #7be091;
    }

    div {
        width: 100%;
        height: 90vh;
        display: inline-block;
        align-items: center;
    }
</style>

<script>
    import { onMount } from 'svelte';
    export let map = new Map();
    let mapTerms = [];
    let display = '';
    let term = '';
    let answerDisplayed = false;
    let currentTerm = 0;

    onMount(() => {
        if (!(map instanceof Map)) throw new TypeError("Map is not instance of map. Please contact a server administator")
        mapTerms = Array.from(map.keys());
        nextQuestion();
    });

    function displayAnswer(){
        if (!(map instanceof Map)) throw new TypeError("Map is not instance of map. Please contact a server administator")
        display = map.get(term);
    }

    function onKeyPress(e) {
        if (e.keyCode == 32){
            if (answerDisplayed){
                answerDisplayed = false;
                nextQuestion();
            } else {
                answerDisplayed = true;
                displayAnswer();
            }
        }
    }

    function nextQuestion() {
        currentTerm+=1;
        if (currentTerm > mapTerms.length - 1) currentTerm = 0;
        term = mapTerms[currentTerm];
        display = term;
    }
</script>

<div>
    <p>{display}</p>
</div>

<svelte:window on:keypress|preventDefault={onKeyPress}/>