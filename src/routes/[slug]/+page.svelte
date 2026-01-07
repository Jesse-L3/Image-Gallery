<script>
    import { slidecontent } from "$lib/data.js";
    import { page } from "$app/stores";
    import { onMount } from "svelte";
    import { gsap } from "gsap";

    $: slug = $page.params.slug;
    $: item = slidecontent.find(i => i.slug === slug);
    
    let xory = Math.random() > 0.5 ? 'x' : 'y';
    console.log(xory);

    let title;
    let title2;
    const tl = gsap.timeline();

    onMount(() => {
    tl.fromTo(
        [title],
        { [xory]: 40, opacity: 0 },
		{ [xory]: 0, opacity: 1, duration: 0.8, ease: 'power3.out' }, '+=1.5'
    )
    .fromTo(
        [title2],
        { x: -40, opacity: 0 },
        { x: 0, opacity: 1, duration: 0.8, ease: 'power3.out' },
        '-=0.4' 
    )
    });
    
</script>

{#if item}
<div>
    <div class="text-container">
        <h1 bind:this={title}>{item.title}</h1>
        <h2 bind:this={title2}>{item.description}</h2>
    </div>
    <img id="to" style="view-transition-name: image-{item.slug}" src="{item.image}" alt="">
</div>
{/if}

<style>
    div{
        display: flex;
        align-items: center;
        justify-content: center;
    }

    @property --fill-color {
        syntax: '<color>';
        inherits: false;
        initial-value: rgba(0, 0, 0, 0);
    }


    div::after{
        content: '';
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background: var(--fill-color);
        z-index: 1;
        animation: fillAnimation 500ms forwards;
        animation-delay: 0.5s;
    }

    .text-container{
        position: absolute;
        z-index: 3;
        display: flex;
        flex-direction: column;
        
    }

    h1,h2{
        z-index: 3;
        text-align: center;
    }

    img {
        width: 100vw;
        height: 100vh;
        object-fit: cover;
    }

    @keyframes fillAnimation {
        to {
            --fill-color: rgba(0, 0, 0, 0.5);
        }
    }

</style>