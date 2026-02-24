<script>
    import { slidecontent } from "$lib/data";
    import {onMount} from "svelte";
    import { gsap } from "gsap";

    let { altcolor = false,
         secondcolor = 'green',
     } = $props();

     let altactive = $derived(altcolor);

    const handleTheme = () => {
        altactive = !altactive;
        const color = altactive ? (altcolor || 'black') : 'white';
        document.documentElement.style.setProperty('--altcolor', color);
    }



    onMount(() => {

    const image = document.querySelectorAll('img');

		const observer = new IntersectionObserver((entries) => { 
        entries.forEach(entry => {
        if (entry.isIntersecting) {
                    const tl = gsap.timeline();
                    let xory = Math.random() > 0.5 ? 'x' : 'y';
                    console.log(xory);
                    let negativeornot = Math.random() > 0.5 ? '-' : ''
                    console.log(negativeornot);

					tl.fromTo(
						entry.target,
						{ [xory]: `${negativeornot}40`, opacity: 0 },
						{ [xory]: `${negativeornot}0`, opacity: 1, duration: 0.8, ease: 'power3.out' }
					);
        } else {
        gsap.killTweensOf(entry.target);
        gsap.set(entry.target, {opacity: 0, x: 0, y: 0, ease: 'power3.out'})
        }
    });               
		}, { threshold: 0.1 });

		image.forEach(img => observer.observe(img));

		return () => observer.disconnect();
	});

</script>

<button onclick={handleTheme}>change theme</button>

<h3>here is the gallary</h3>

    <ul>
        {#each slidecontent as slide}
        <li>
            <a href="/{slide.slug}"><img loading="lazy" src="{slide.image}" alt=""  style="view-transition-name: image-{slide.slug}"></a>
        </li>
        {/each}
    </ul>
    <!-- <ul aria-hidden=true>
        {#each slidecontent as slide, index}
        <li>
            <a href="/{slide.slug}"><img src="{slide.image}" alt=""  style="view-transition-name: image-{slide.slug + index}"></a>
        </li>
        {/each}
    </ul> -->

<!-- create anothe carrousel but with different images  and use index for the aria hidden true-->

<!-- <div class="carrousel">
    <ul>
        {#each slidecontent as slide}
        <li>
            <a href="/{slide.slug}"><img src="{slide.image}" alt=""  style="view-transition-name: image-{slide.slug}"></a>
        </li>
        {/each}
    </ul>
    <ul aria-hidden=true>
        {#each slidecontent as slide}
        <li>
            <a href="/{slide.slug}"><img src="{slide.image}" alt=""  style="view-transition-name: image-{slide.slug}"></a>
        </li>
        {/each}
    </ul>
</div>
<div class="carrousel">
    <ul>
        {#each slidecontent as slide}
        <li>
            <a href="/{slide.slug}"><img src="{slide.image}" alt=""  style="view-transition-name: image-{slide.slug}"></a>
        </li>
        {/each}
    </ul>
    <ul aria-hidden=true>
        {#each slidecontent as slide}
        <li>
            <a href="/{slide.slug}"><img src="{slide.image}" alt=""  style="view-transition-name: image-{slide.slug}"></a>
        </li>
        {/each}
    </ul>
</div> -->

<style>

:root{
    --altcolor: black;
}

h3{
    text-align: center;
    margin: 2em 0;
    color: var(--altcolor);
}
a {
  text-decoration: none;
  pointer-events: all;
  z-index: 4;
}


ul{
  columns: 3 300px;
  column-gap: 1em;
  width: min(1000px, 100%);
  margin: 0 auto;
}


li {
  list-style: none;
  width: 300px;
  display: block;
  position: relative;
  margin-bottom: 1em;
}


img{
  width: 100%;
  object-fit: cover;
  opacity: 0;
}

</style>
