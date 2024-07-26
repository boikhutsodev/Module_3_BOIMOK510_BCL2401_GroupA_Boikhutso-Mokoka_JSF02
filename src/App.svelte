<script>
    import { Router, Route, link } from 'svelte-routing';
    import Navbar from './components/Navbar.svelte';
    import ProductGrid from './components/ProductGrid.svelte';
    import ProductDetail from './components/ProductDetail.svelte';
    import Loading from './components/Loading.svelte';
    
    import { writable, derived } from 'svelte/store';
    import { onMount } from 'svelte';
  
    const products = writable([]);
    const categories = writable([]);
    const searchQuery = writable('');
    const selectedCategory = writable('');
    const sortOrder = writable('');
    const loading = writable(true);
  
    const fetchProducts = async () => {
      loading.set(true);
      const response = await fetch('https://fakestoreapi.com/products');
      const data = await response.json();
      products.set(data);
      loading.set(false);
    };
  
    const fetchCategories = async () => {
      const response = await fetch('https://fakestoreapi.com/products/categories');
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
          prods = prods.filter((product) => product.category === $selectedCategory);
        }
        if ($sortOrder === 'asc') {
          prods = prods.sort((a, b) => a.price - b.price);
        } else if ($sortOrder === 'desc') {
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
    <div class="container mx-auto p-6"></div>
</Router>