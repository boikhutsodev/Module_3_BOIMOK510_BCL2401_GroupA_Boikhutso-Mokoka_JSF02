<script>
    import { link } from 'svelte-routing';
    export let filteredProducts;
  </script>
  
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
    {#each $filteredProducts as product}
      <div class="card border p-4 rounded shadow hover:shadow-lg transition-shadow duration-300 cursor-pointer">
        <a use:link href={`/product/${product.id}`} class="block">
          <img src={product.image} alt={product.title} class="w-400px h-48 object-cover mb-4 rounded" />
          <h3 class="text-lg font-bold mb-2">{product.title}</h3>
          <p class="text-gray-700 mb-2">${product.price}</p>
          <p class="text-gray-500">{product.category}</p>
        </a>
      </div>
    {/each}
  </div>
  