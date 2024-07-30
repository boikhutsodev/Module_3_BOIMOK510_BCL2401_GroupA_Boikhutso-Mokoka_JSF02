<script>
  import { onMount } from "svelte";
  import { writable } from "svelte/store";

  export let params;
  const product = writable(null);
  const loading = writable(true);
  const favorites = writable([]);

  onMount(async () => {
    loading.set(true);
    const response = await fetch(
      `https://fakestoreapi.com/products/${params.id}`
    );
    const data = await response.json();
    product.set(data);
    loading.set(false);
  });

  onMount(() => {
    const storedFavorites = localStorage.getItem("favorites");
    if (storedFavorites) {
      favorites.set(JSON.parse(storedFavorites));
    }
  });

  function goBack() {
    window.history.back();
  }

  function toggleFavorite(productId) {
    favorites.update((currentFavorites) => {
      const newFavorites = currentFavorites.includes(productId)
        ? currentFavorites.filter((id) => id !== productId)
        : [...currentFavorites, productId];
      localStorage.setItem("favorites", JSON.stringify(newFavorites));
      return newFavorites;
    });
  }

  function isFavorite(productId) {
    let isFav;
    favorites.subscribe((favs) => {
      isFav = favs.includes(productId);
    })();
    return isFav;
  }
</script>

{#if $loading}
  <div
    class="fixed inset-0 flex items-center justify-center bg-gray-100 bg-opacity-75"
  >
    <div class="text-center py-8 bg-white p-4 rounded shadow-md">
      <svg
        class="w-16 h-16 mx-auto animate-spin"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
      >
        <circle
          class="opacity-25"
          cx="12"
          cy="12"
          r="10"
          stroke="currentColor"
          stroke-width="4"
        ></circle>
        <path
          class="opacity-75"
          fill="currentColor"
          d="M4 12c0-4.418 3.582-8 8-8s8 3.582 8 8H4z"
        ></path>
      </svg>
      <p class="mt-4">Loading...</p>
    </div>
  </div>
{:else}
  <div class="container mx-auto p-6">
    {#if $product}
      <div class="bg-white p-6 rounded shadow-lg mt-20">
        <button on:click={goBack} class="bg-pink-600"> Go Back </button>
        <img
          src={$product.image}
          alt={$product.title}
          class="w-400px h-64 object-cover mb-4 rounded"
        />
        <h3 class="text-2xl font-bold mb-2">{$product.title}</h3>
        <p class="text-gray-700 mb-2">${$product.price}</p>
        <p class="text-gray-500 mb-4">Category: {$product.category}</p>
        <p class="text-gray-700 mb-4">
          Rating: {$product.rating.rate} ({$product.rating.count} reviews)
        </p>
        <p class="text-gray-700">{$product.description}</p>
        <div class="space-y-4 space-x-4">
          <button on:click={() => toggleFavorite($product.id)} class="mr-10px">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="currentColor"
              class="w-6 h-6"
              class:text-red-500={isFavorite($product.id)}
              class:text-gray-300={!isFavorite($product.id)}
              viewBox="0 0 24 24"
            >
              <path
                d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
              />
            </svg>
          </button>
          <button
            class="bg-pink-500 hover:bg-pink-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring-2 focus:ring-pink-400 focus:ring-opacity-75 transition duration-200"
          >
            Add To Cart +
          </button>
        </div>
      </div>
    {/if}
  </div>
{/if}
