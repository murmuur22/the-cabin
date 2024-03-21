<script>
    import { onMount } from 'svelte';
    import { blur, fade } from 'svelte/transition';


    $: stage = 1;

    let stage1_ref;
    let stage2_ref;
    let time;
    let duration;
    const playing = (curr_time) => {
        if (curr_time >= duration/2){ // THIS WILL EXECUTE IF THE MOUSE COMES BACK IN, PAUSING THE VIDEO AT THE WRONG TIME
            
            // reached apex
            stage = 2;
        }
    }
    $: playing(time);


    let isButtonVisible = true;
    function handleStart() {
        stage1_ref.play();
        isButtonVisible = false;
    }

    let pooped = false;
    function handlePoop() {
        stage = 1;
        isButtonVisible = true;
        pooped = true;
        console.log(stage);
    }

</script>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<!-- svelte-ignore a11y-mouse-events-have-key-events -->
<div
    style="background-image: url('/assets/end_frame.jpeg')"
    class="
        bg-center bg-cover blur
        h-screen w-screen
    "
/>
<div
    class = "
        absolute top-0 left-0
        flex justify-center items-center
        h-screen w-screen
    "
>
    {#if stage == 1}
        <video src="/assets/Intro.mp4" playsinline muted bind:this={stage1_ref} bind:currentTime={time} bind:duration
            class="
                drop-shadow-lg
                rounded-lg
                w-10/12
            "
        />
        {#if isButtonVisible == true}
            {#if pooped == true}
                <button out:blur on:click={handleStart} class="absolute rounded-full bg-stone-950 text-stone-50 px-6 py-2 hover:scale-125 transition ease-in-out">BUT YOU POOPED THE PARTY</button>
            {:else}
                <button out:blur on:click={handleStart} class="absolute rounded-full bg-stone-950 text-stone-50 px-6 py-2 hover:scale-125 transition ease-in-out">PARTY TIME!!</button>
            {/if}
            
        {/if}
    {/if}

    {#if stage == 2}
        <video in:fade src="/assets/LASERS.mp4" autoplay loop playsinline muted bind:this={stage2_ref}
            class="
                drop-shadow-lg
                rounded-lg
                w-10/12
            "
        />
        <button in:blur on:click={handlePoop} class="absolute rounded-full bg-stone-950 text-stone-50 px-6 py-2 hover:scale-125 transition ease-in-out">stop the party</button>
    {/if}
</div>