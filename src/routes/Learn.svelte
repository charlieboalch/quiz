<style>
    .centered {
        width: 100%;
        text-align: center;
    }

    .title {
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: 40px;
        padding-top: 5%;
        display: block;
        font-weight: 300;
    }

    input {
        display: inline;
    }

    p {
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: large;
        display: block;
        font-weight: 300;
    }
</style>

<script>
    import { createEventDispatcher, onMount } from 'svelte';
    import stringSimilarity from 'string-similarity'

    const dispatch = createEventDispatcher();
    export let map;

    let prompt = '';
    let answer = '';
    let input = '';
    let shouldShowAnswer = false;

    function submitAnswer() {
        if (stringSimilarity.compareTwoStrings(answer.toLowerCase(), input.toLowerCase()) >= 0.80){
            prompt = "Correct!";
        } else {
            prompt = "Incorrect :(";
            shouldShowAnswer = true;
        }

        setTimeout(() => {
            nextQuestion();
        }, 3000);
    }

    //init logic
    onMount(() => {
        if (!(map instanceof Map)){
            throw new TypeError("Map is incorrect type, contact server administrator");
        }

        nextQuestion();
    });

    function nextQuestion() {
        if (!(map instanceof Map)) return;
        input = '';
        shouldShowAnswer = false;

        let correct = Math.floor(Math.random() * map.size);
        let temp = Array.from(map.keys());

        prompt = temp[correct];
        answer = map.get(temp[correct]);
        console.log(answer);
    }
</script>

<div class="centered">
    <h1 class="title">{prompt}</h1>
    {#if shouldShowAnswer}
        <p>{answer}</p>
    {/if}
    <div>
        <input bind:value="{input}">
        <button on:click={submitAnswer}>Submit</button>
    </div>
</div>