<script>
  import { Router, Route, link } from "svelte-routing";
  import Navbar from "./components/Navbar.svelte";
  import ProductGrid from "./components/ProductGrid.svelte";
  import ProductDetail from "./components/ProductDetail.svelte";
  import Loading from "./components/Loading.svelte";

  import { writable, derived } from "svelte/store";
  import { onMount } from "svelte";

  const products = writable([]);
  const categories = writable([]);
  const searchQuery = writable("");
  const selectedCategory = writable("");
  const sortOrder = writable("");
  const loading = writable(true);

  const fetchProducts = async () => {
    loading.set(true);
    const response = await fetch("https://fakestoreapi.com/products");
    const data = await response.json();
    products.set(data);
    loading.set(false);
  };

  const fetchCategories = async () => {
    const response = await fetch(
      "https://fakestoreapi.com/products/categories"
    );
    const data = await response.json();
    categories.set(data);
  };

  const searchProducts = () => {
    products.subscribe((prods) => {
      const result = prods.filter((product) =>
        product.title.toLowerCase().includes(searchQuery.get().toLowerCase())
      );
      filteredProducts.set(result);
    });
  };

  const filteredProducts = derived(
    [products, selectedCategory, sortOrder, searchQuery],
    ([$products, $selectedCategory, $sortOrder, $searchQuery]) => {
      let prods = $products;
      if ($selectedCategory) {
        prods = prods.filter(
          (product) => product.category === $selectedCategory
        );
      }
      if ($sortOrder === "asc") {
        prods = prods.sort((a, b) => a.price - b.price);
      } else if ($sortOrder === "desc") {
        prods = prods.sort((a, b) => b.price - a.price);
      }
      if ($searchQuery) {
        prods = prods.filter((product) =>
          product.title.toLowerCase().includes($searchQuery.toLowerCase())
        );
      }
      return prods;
    }
  );

  onMount(() => {
    fetchProducts();
    fetchCategories();
  });
</script>

<Navbar />

<Router>
  <Route path="/" exact>
    <div class="container mx-auto p-6">
      <div class="mt-20 flex justify-between items-center flex-wrap mb-4">
        <select
          bind:value={$selectedCategory}
          class="border p-2 rounded mb-2 sm:mb-0"
        >
          <option value="">All Categories</option>
          {#each $categories as category}
            <option value={category}>{category}</option>
          {/each}
        </select>

        <div class="flex items-center mb-2 sm:mb-0">
          <input
            type="text"
            bind:value={$searchQuery}
            placeholder="Search products..."
            class="border p-2 rounded-l"
          />
          <button
            on:click={searchProducts}
            class="bg-white text-black border border-l-0 p-2 rounded-r"
            >Search</button
          >
        </div>

        <select bind:value={$sortOrder} class="border p-2 rounded">
          <option value="">Sort by Price</option>
          <option value="asc">Lowest to Highest</option>
          <option value="desc">Highest to Lowest</option>
        </select>
      </div>

      {#if $loading}
        <Loading />
      {:else}
        <ProductGrid {filteredProducts} />
      {/if}
    </div>
  </Route>
  <Route path="/product/:id" let:params>
    <ProductDetail {params} />
  </Route>
</Router>
