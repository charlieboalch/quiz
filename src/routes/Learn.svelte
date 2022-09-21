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
    import Denque from 'denque';

    const dispatch = createEventDispatcher();
    export let map;

    let prompt = '';
    let answer = '';
    let input = '';
    let shouldShowAnswer = false;
    let queue;

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

        let tempTerms = Array.from(map.keys());
        queue = new Denque(shuffleArray(tempTerms));

        nextQuestion();
    });

    function shuffleArray(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
        return array;
    }

    function nextQuestion() {
        if (!(map instanceof Map)) return;
        if (queue.isEmpty()){
            let tempTerms = Array.from(map.keys());
            queue = new Denque(shuffleArray(tempTerms));
        }
        input = '';
        shouldShowAnswer = false;

        prompt = queue.shift();
        answer = map.get(prompt);
    }
</script>

<div class="centered">
    <h1 class="title">{prompt}</h1>
    {#if shouldShowAnswer}
        <p>{answer}</p>
    {/if}
    <div>
        <form onsubmit="return false">
            <input bind:value="{input}">
            <button on:click={submitAnswer}>Submit</button>
        </form>
    </div>
</div>