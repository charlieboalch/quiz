<style>
    .centered {
        width: 100%;
        text-align: center;
    }

    .title {
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: 50px;
        padding-top: 5%;
        display: block;
        font-weight: 300;
    }

    input {
        display: inline;
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

    function submitAnswer() {
        if (stringSimilarity.compareTwoStrings(answer, input) >= 0.80){
            prompt = "Correct!";
        } else {
            prompt = "Incorrect :(";
        }

        setTimeout(() => {
            nextQuestion();
        }, 2000);
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

        let correct = Math.floor(Math.random() * map.size);
        let temp = Array.from(map.keys());

        prompt = temp[correct];
        answer = map.get(temp[correct]);
        console.log(answer);
    }
</script>

<div class="centered">
    <h1 class="title">{prompt}</h1>
    <div>
        <input bind:value="{input}">
        <button on:click={submitAnswer}>Submit</button>
    </div>
</div>