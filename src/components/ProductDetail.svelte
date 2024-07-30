<script>
  // Import necessary functions from Svelte
  import { onMount } from "svelte";
  import { writable } from "svelte/store";

  // Declare component props and state variables
  export let params; // Expects an object with `id` property from the parent component
  const product = writable(null); // Store for the product data
  const loading = writable(true); // Store to manage loading state
  const favorites = writable([]); // Store to manage the list of favorite product IDs

  // Fetch product data when the component is mounted
  onMount(async () => {
    loading.set(true); // Set loading to true while fetching data
    const response = await fetch(
      `https://fakestoreapi.com/products/${params.id}` // Fetch the product data by ID from the API
    );
    const data = await response.json(); // Parse the JSON data
    product.set(data); // Update the product store with the fetched data
    loading.set(false); // Set loading to false after data is fetched
  });

  // Load favorites from localStorage when the component is mounted
  onMount(() => {
    const storedFavorites = localStorage.getItem("favorites"); // Retrieve favorites from localStorage
    if (storedFavorites) {
      favorites.set(JSON.parse(storedFavorites)); // Parse and set the favorites store
    }
  });

  // Function to navigate back in history
  function goBack() {
    window.history.back(); // Go back to the previous page in browser history
  }

  // Function to toggle a product's favorite status
  function toggleFavorite(productId) {
    favorites.update((currentFavorites) => {
      // Check if the product is already in favorites
      const newFavorites = currentFavorites.includes(productId)
        ? currentFavorites.filter((id) => id !== productId) // Remove if already favorited
        : [...currentFavorites, productId]; // Add if not favorited
      localStorage.setItem("favorites", JSON.stringify(newFavorites)); // Save updated favorites to localStorage
      return newFavorites; // Return the updated favorites list
    });
  }

  // Function to check if a product is in favorites
  function isFavorite(productId) {
    let isFav;
    favorites.subscribe((favs) => {
      isFav = favs.includes(productId); // Check if productId is in favorites list
    })();
    return isFav; // Return whether the product is a favorite
  }
</script>

<!-- Display a loading spinner if data is still being fetched -->
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
  <!-- Display the product details once loading is complete -->
  <div
    class="container flex justify-center items-center p-6 min-h-screen m-auto"
  >
    {#if $product}
      <div class="bg-white p-6 rounded shadow-lg mt-5">
        <!-- Button to go back to the previous page -->
        <button
          on:click={goBack}
          class="fixed bg-orange-400 hover:bg-pink-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:ring-2 focus:ring-pink-300 focus:ring-opacity-75 transition duration-200"
        >
          Go Back
        </button>
        <!-- Product image -->
        <div class="flex justify-center items-center mt-5">
          <img
            src={$product.image}
            alt={$product.title}
            class="w-400px h-64 object-cover mb-4 rounded"
          />
        </div>
        <!-- Product title, price, category, and description -->
        <h3 class="text-2xl font-bold mb-2">{$product.title}</h3>
        <p class="text-gray-700 mb-2">${$product.price}</p>
        <p class="text-gray-500 mb-2">Category: {$product.category}</p>
        <p class="text-gray-700 mb-2">
          Rating: {$product.rating.rate} ({$product.rating.count} reviews)
        </p>
        <p class="text-gray-700">{$product.description}</p>
        <!-- Buttons to toggle favorite status and add product to cart -->
        <div class="flex justify-evenly items-center mt-5">
          <!-- Toggle favorite status button -->
          <button on:click={() => toggleFavorite($product.id)} class="">
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
          <!-- Button to add product to cart (no functionality in this snippet) -->
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
