<script>
    import {createEventDispatcher} from 'svelte';
    import ProgressBar from './ProgressBar.svelte';

    const totalSeconds = 20;
    let secondsLeft = totalSeconds;
    let isRunning = false;
    const dispatch = createEventDispatcher();

    $: progress= ((totalSeconds-secondsLeft)/totalSeconds)*100;

    function handleClick() {        
        isRunning = true;
        const id = setInterval(() => {
            secondsLeft--;
            if (secondsLeft===0) {
                clearInterval(id);
                isRunning=false;
                progress=0;
                dispatch('end');
            }
        }, 1000);
    }
</script>
<style>
    h2 {
        margin:0;
    }
    .start {
        background-color:rgb(154,73,73);
        color:white;
        width:100%;
        margin:10px 0;
    }
    .start[disabled] {
        background-color:rgb(194,194,194);
        cursor:not-allowed;
    }

</style>

<div bp="grid">
    <h2 bp="offset-5@md 4@md 12@sm">
        Seconds Left: {secondsLeft}
    </h2>
</div>
<ProgressBar {progress}/>
<div bp="grid">
    <button disabled={isRunning} bp="offset-5@md 4@md 12@sm" class="start"
     on:click={handleClick}
    >
        Start
    </button>
</div>