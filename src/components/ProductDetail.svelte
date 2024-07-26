<script>
    import { onMount } from 'svelte';
    import { writable } from 'svelte/store';
  
    export let params;
    const product = writable(null);
    const loading = writable(true);
  
    onMount(async () => {
      loading.set(true);
      const response = await fetch(`https://fakestoreapi.com/products/${params.id}`);
      const data = await response.json();
      product.set(data);
      loading.set(false);
    });
  </script>
  
  {#if $loading}
  <div class="fixed inset-0 flex items-center justify-center bg-gray-100 bg-opacity-75">
    <div class="text-center py-8 bg-white p-4 rounded shadow-md">
      <svg class="w-16 h-16 mx-auto animate-spin" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
        <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
        <path class="opacity-75" fill="currentColor" d="M4 12c0-4.418 3.582-8 8-8s8 3.582 8 8H4z"></path>
      </svg>
      <p class="mt-4">Loading...</p>
    </div>
  </div>
  {:else}
    <div class="container mx-auto p-6">
      {#if $product}
        <div class="bg-white p-6 rounded shadow-lg mt-20">
          <img src={$product.image} alt={$product.title} class="w-400px h-64 object-cover mb-4 rounded" />
          <h3 class="text-2xl font-bold mb-2">{$product.title}</h3>
          <p class="text-gray-700 mb-2">${$product.price}</p>
          <p class="text-gray-500 mb-4">Category: {$product.category}</p>
          <p class="text-gray-700 mb-4">Rating: {$product.rating.rate} ({$product.rating.count} reviews)</p>
          <p class="text-gray-700">{$product.description}</p>
        </div>
      {/if}
    </div>
  {/if}
  