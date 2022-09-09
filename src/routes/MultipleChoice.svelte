<script>
    import { createEventDispatcher, onMount } from 'svelte';
    import Denque from 'denque'
    export let map;
    let prompt = '';
    let answer = '';
    let button1 = '';
    let button2 = '';
    let button3 = '';
    let button4 = '';
    let shouldShowAnswer = false;
    let queue;

    const dispatch = createEventDispatcher();

    function shuffleArray(array) {
        for (let i = array.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [array[i], array[j]] = [array[j], array[i]];
        }
        return array;
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

    function nextQuestion() {
        if (!(map instanceof Map)) return;
        if (queue.isEmpty()){
            let tempTerms = Array.from(map.keys());
            queue = new Denque(shuffleArray(tempTerms));
            alert(queue.toArray().toString());
        }
        button1 = '';
        button2 = '';
        button3 = '';
        button4 = '';
        shouldShowAnswer = false;
        
        prompt = queue.shift();
        answer = map.get(prompt);

        let pos = Math.floor(Math.random() * 4);
        let temp = Array.from(map.keys());
        switch (pos) {
            case 0:
                button1 = answer;
                button2 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                button3 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                button4 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                break;
            case 1:
                button2 = answer;
                button1 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                button3 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                button4 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                break;
            case 2:
                button3 = answer;
                button2 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                button1 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                button4 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                break;
            case 3:
                button4 = answer;
                button2 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                button3 = map.get(temp[Math.floor(Math.random() * temp.length)]);
                button1 = map.get(temp[Math.floor(Math.random() * temp.length)]);    
                break;
        }
    }

    function onButtonClick(arg){
        if (arg == answer){
            prompt = "Correct!";
        } else {
            prompt = "Incorrect :(";
            shouldShowAnswer = true;
        }

        setTimeout(() => {
            nextQuestion();
        }, 2500);
    }
</script>

<style>
    .centered {
        width: 100%;
        text-align: center
    }

    p {
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: large;
        font-weight: 300;
        display: block;
    }

    .title {
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        font-size: 40px;
        padding-top: 5%;
        font-weight: 300;
        margin-left: 10%;
        margin-right: 10%;
        display: block;
    }

    .option {
        width: 15%;
        font-size: large;
    }
</style>

<div class="centered">
    <h1 class="title">{prompt}</h1>
    {#if shouldShowAnswer}
        <p>{answer}</p>  
    {/if}
</div>

<div class="centered">
    <button class="option" on:click={() => onButtonClick(button1)}>{button1}</button>
    <button class="option" on:click={() => onButtonClick(button2)}>{button2}</button>
    <button class="option" on:click={() => onButtonClick(button3)}>{button3}</button>
    <button class="option" on:click={() => onButtonClick(button4)}>{button4}</button>
</div>