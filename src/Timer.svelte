<script>
    import {createEventDispatcher} from "svelte";
    import ProgressBar from './ProgressBar.svelte';
    const totalSecond = 20;
    let secondLeft = totalSecond;
    let disabledRunning = false;
    $: progress = (totalSecond - secondLeft)/totalSecond * 100;
    const dispatch  = createEventDispatcher();
    function startTimer(){
        disabledRunning = true;
        const timer = setInterval(() => {
        secondLeft -=1;
        if(secondLeft <= 0 ){
            clearInterval(timer);
            disabledRunning = false;
            secondLeft = totalSecond;
            dispatch('end');
            }
        else if (secondLeft == totalSecond){
            disabledRunning = true;
        }
        },1000);
    }
    
</script>

<style>
    h2{
        margin: 0;
    }
    .start{
        background-color: rgb(154, 73, 73);
        width: 100%;
        margin: 10px 0;
        color: white;
    }
    .start[disabled]{
        background-color: gray;
        cursor: not-allowed;
    }
</style>
<div bp="grid">
    <h2 bp="offset-5@md 4@md 12@sm">
        Second Left: {secondLeft}
    </h2>
</div>
<ProgressBar {progress}/>
<div bp="grid">
    <button disabled={disabledRunning} on:click={startTimer} bp="offset-5@md 4@md 12@sm" class="start">
        {#if disabledRunning==true } Please Dont Refresh {:else }Start {/if}
    </button>
</div>
