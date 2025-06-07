<script>
  import { onMount } from "svelte";
  import placeholder from "./assets/svelte.svg";

  let vendors = [];
  let products = [];
  let loading = true;
  let error = "";
  let showProducts = true;

  // Replace with your actual WordPress site URL
  const API_URL_VENDORS =
    "http://amointg.cluster023.hosting.ovh.net/wp-json/dokan/v1/stores";
  const API_URL_PRODUCTS =
    "http://amointg.cluster023.hosting.ovh.net/wp-json/wc/v3/products";

  async function fetchVendors() {
    loading = true;
    error = "";
    try {
      // Fetch vendors from the API
      // Use Basic Auth for authentication
      const headers = new Headers();
      headers.append("Authorization", "Basic " + btoa("ck_28c6c1217148588523c66fd3e4390ddc59daa528:cs_bbca6726877363d0ef7ff0b1d0cb9ce06aeb49c3"));
      headers.append("Content-Type", "application/json");
      const res = await fetch(API_URL_VENDORS, {
        method: "GET",
        headers: headers,
      });
      if (!res.ok) throw new Error("Failed to fetch vendors");
      vendors = await res.json();
    } catch (e) {
      error = e.message;
    } finally {
      loading = false;
    }
  }

  async function fetchProducts() {
    loading = true;
    error = "";
    try {
      const res = await fetch(API_URL_PRODUCTS);
      if (!res.ok) throw new Error("Failed to fetch products");
      products = await res.json();
    } catch (e) {
      error = e.message;
    } finally {
      loading = false;
    }
  }

  function switchView(view) {
    showProducts = view === "products";
    if (showProducts) {
      if (products.length === 0) {
        fetchProducts();
      }
    } else {
      if (vendors.length === 0) {
        fetchVendors();
      }
    }
  }

  onMount(fetchProducts);
</script>

<div class="flex justify-center items-center flex-col py-3">

  <!-- Switch between products and vendors -->
  <div class="switch flex gap-1 rounded-full">
    <button class:active={showProducts} on:click={() => switchView("products")}
      >prodotti</button
    >
    <button class:active={!showProducts} on:click={() => switchView("vendors")}
      >venditori</button
    >
  </div>

  <!-- Products and Vendors grid -->
  {#if loading}
    <!-- <p class="text-lg text-gray-500 text-center my-8">
    Loading {showProducts ? "vendors" : "products"}...
  </p> -->
  {:else if error}
    <p class="text-red-600 text-center my-8">{error}</p>
  {/if}
  <div
    class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-5 mt-6"
  >
    {#if showProducts}
      {#each products as product}
        <div
          class="product-card bg-white rounded-xs flex-col items-center" 
        >
          <img
            class="product-img w-2xs h-52 object-cover mb-3"
            src={product.images?.[0]?.src || placeholder}
            alt={product.name}
          />
          <p class="text-sm text-gray-800">
            {product.title.rendered}
          </p>
          
          <div class="text-lg font-semibold text-gray-800">{product.name}</div>
        </div>
      {/each}
    {:else}
      {#each vendors as vendor}
        <div
          class="vendor-card bg-white rounded-xl shadow hover:shadow-lg transition p-6 flex flex-col items-center"
        >
          <img
            class="vendor-img w-20 h-20 rounded-full object-cover mb-4 border-2 border-blue-200"
            src={vendor.gravatar || vendor.avatar || placeholder}
            alt={vendor.store_name}
          />
          <div class="text-lg font-semibold text-gray-800">
            {vendor.store_name}
          </div>
        </div>
      {/each}
    {/if}
  </div>
</div>

<style>
  .switch {
    background-color: #f8f9fa;
    border-radius: 50px;
    padding: 6px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .switch button {
    padding: 8px 20px;
    border-radius: 50px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    font-size: 28px;
  }

  .switch button.active {
    background-color: #ffffff;
    color: black;
  }

  .vendor-card,
  .product-card {
    transition:
      transform 0.2s ease,
      box-shadow 0.2s ease;
  }

  .vendor-card:hover,
  .product-card:hover {
    box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 12px;
  }
</style>
