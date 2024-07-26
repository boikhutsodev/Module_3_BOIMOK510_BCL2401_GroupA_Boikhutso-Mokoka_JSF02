<script>
    import { writable } from 'svelte/store';
  
    export let open = writable(false);
  
    const toggleMenu = () => {
      open.update((n) => !n);
    };
  </script>
  
  <nav class="fixed w-full bg-pink-600 p-4 text-pink-50 shadow-lg">
    <div class="container mx-auto flex justify-between items-center">
      <div class="text-lg font-bold">My FlexStore</div>
      <div class="hidden md:flex">
        <a href="/" class="mr-4 hover:underline">Home</a>
        <a href="./" class="mr-4 hover:underline">Wishlist</a>
        <a href="./" class="mr-4 hover:underline">
          <svg width="32px" height="32px" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
            <!-- SVG content -->
          </svg>
        </a>
        <a href="./" class="hover:underline">Login</a>
      </div>
      <button on:click={toggleMenu} class="md:hidden text-white">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" class="w-6 h-6">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7" />
        </svg>
      </button>
    </div>
    {#if $open}
      <div class="md:hidden mt-4 bg-pink-500">
        <a href="./" class="block p-2 hover:bg-pink-400">Wishlist</a>
        <a href="./" class="block p-2 hover:bg-pink-400">
          <svg width="32px" height="32px" viewBox="0 0 32 32" fill="none" xmlns="http://www.w3.org/2000/svg">
            <!-- SVG content -->
          </svg>
        </a>
        <a href="./" class="block p-2 hover:bg-pink-400">Login</a>
      </div>
    {/if}
  </nav>
  