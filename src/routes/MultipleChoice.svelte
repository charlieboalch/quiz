<script>
    import { createEventDispatcher, onMount } from 'svelte';
    export let map;
    let prompt = '';
    let answer = '';
    let button1 = '';
    let button2 = '';
    let button3 = '';
    let button4 = '';

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

        nextQuestion();
    });

    function nextQuestion() {
        if (!(map instanceof Map)) return;
        button1 = '';
        button2 = '';
        button3 = '';
        button4 = '';
        let correct = Math.floor(Math.random() * map.size);

        let temp = Array.from(map.keys());

        prompt = temp[correct];
        answer = map.get(temp[correct]);

        let pos = Math.floor(Math.random() * 4);
        console.log(pos);
        switch (pos) {
            case 0:
                button1 = answer;
                button2 = map.get(temp[Math.floor(Math.random() * map.size)]);
                button3 = map.get(temp[Math.floor(Math.random() * map.size)]);
                button4 = map.get(temp[Math.floor(Math.random() * map.size)]);
                break;
            case 1:
                button2 = answer;
                button1 = map.get(temp[Math.floor(Math.random() * map.size)]);
                button3 = map.get(temp[Math.floor(Math.random() * map.size)]);
                button4 = map.get(temp[Math.floor(Math.random() * map.size)]);
                break;
            case 2:
                button3 = answer;
                button2 = map.get(temp[Math.floor(Math.random() * map.size)]);
                button1 = map.get(temp[Math.floor(Math.random() * map.size)]);
                button4 = map.get(temp[Math.floor(Math.random() * map.size)]);
                break;
            case 3:
                button4 = answer;
                button2 = map.get(temp[Math.floor(Math.random() * map.size)]);
                button3 = map.get(temp[Math.floor(Math.random() * map.size)]);
                button1 = map.get(temp[Math.floor(Math.random() * map.size)]);    
                break;
        }
    }

    function onButtonClick(arg){
        if (arg == answer){
            prompt = "Correct!";
        } else {
            prompt == "Incorrect :(";
        }

        setTimeout(() => {
            nextQuestion();
        }, 1500);
    }

    function returnHome(){
        dispatch("message", {
            text: 'home'
        });
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

    #return {
        margin-top: 5%;
    }
</style>

<h1 class="centered title">{prompt}</h1>

<div class="centered">
    <button class="option" on:click={() => onButtonClick(button1)}>{button1}</button>
    <button class="option" on:click={() => onButtonClick(button2)}>{button2}</button>
    <button class="option" on:click={() => onButtonClick(button3)}>{button3}</button>
    <button class="option" on:click={() => onButtonClick(button4)}>{button4}</button>
</div>

<div class="centered" id="return">
    <button on:click={returnHome}>Return to Home</button>
</div>