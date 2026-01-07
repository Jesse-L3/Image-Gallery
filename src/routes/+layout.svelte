<script>
    import { run } from 'svelte/legacy';
	import { onNavigate } from "$app/navigation";
    import { navigating } from '$app/stores';
	let { children } = $props();


      onNavigate((navigation) => {
    if (!document.startViewTransition) return;

    return new Promise((resolve) => {
      document.startViewTransition(async () => {
        resolve();
        await navigation.complete;
      });
    });
  });
</script>

<header>
	
</header>

<main>
	{@render children()}
</main>

<footer>

</footer>

<style>
	header{
		position: fixed;
		z-index: 2;
		justify-self: center;
	}

  main{
    height: 20000px;
  }
</style>