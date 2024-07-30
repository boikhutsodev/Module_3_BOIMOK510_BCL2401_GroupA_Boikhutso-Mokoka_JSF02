<script>
  import { onMount } from "svelte";
  import { link } from "svelte-routing";
  export let filteredProducts;

  let favorites = [];

  onMount(() => {
    const storedFavorites = localStorage.getItem("favorites");
    if (storedFavorites) {
      favorites = JSON.parse(storedFavorites);
    }
  });

  function toggleFavorite(productId) {
    if (favorites.includes(productId)) {
      favorites = favorites.filter((id) => id !== productId);
    } else {
      favorites.push(productId);
    }
    localStorage.setItem("favorites", JSON.stringify(favorites));
  }

  function isFavorite(productId) {
    return favorites.includes(productId);
  }
</script>

<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
  {#each $filteredProducts as product}
    <div
      class="card border p-4 rounded shadow hover:shadow-lg transition-shadow duration-300 cursor-pointer"
    >
      <a use:link href={`/product/${product.id}`} class="block">
        <img
          src={product.image}
          alt={product.title}
          class="w-400px h-48 object-cover mb-4 rounded"
        />
      </a>
      <h3 class="text-lg font-bold mb-2">{product.title}</h3>
      <p class="text-gray-700 mb-2">${product.price}</p>
      <p class="text-gray-500">{product.category}</p>
      <p class="text-gray-700 mb-4">
        Rating: {product.rating.rate} ({product.rating.count} reviews)
      </p>
      <div class="space-y-4 space-x-4">
        <!-- SVG Favr -->
        <button on:click={() => toggleFavorite(product.id)} class="mr-10px">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="currentColor"
            class="w-6 h-6"
            class:text-red-500={isFavorite(product.id)}
            class:text-gray-300={!isFavorite(product.id)}
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
  {/each}
</div>
