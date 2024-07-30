<script>
  import { writable } from "svelte/store";
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
        <svg
          width="32px"
          height="32px"
          viewBox="0 0 32 32"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <!-- SVG content -->
          <path
            d="M7.5 18C8.32843 18 9 18.6716 9 19.5C9 20.3284 8.32843 21 7.5 21C6.67157 21 6 20.3284 6 19.5C6 18.6716 6.67157 18 7.5 18Z"
            stroke="#1C274C"
            stroke-width="1.5"
          />
          <path
            d="M16.5 18.0001C17.3284 18.0001 18 18.6716 18 19.5001C18 20.3285 17.3284 21.0001 16.5 21.0001C15.6716 21.0001 15 20.3285 15 19.5001C15 18.6716 15.6716 18.0001 16.5 18.0001Z"
            stroke="#1C274C"
            stroke-width="1.5"
          />
          <path
            d="M11 10.8L12.1429 12L15 9"
            stroke="#1C274C"
            stroke-width="1.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
          <path
            d="M2 3L2.26121 3.09184C3.5628 3.54945 4.2136 3.77826 4.58584 4.32298C4.95808 4.86771 4.95808 5.59126 4.95808 7.03836V9.76C4.95808 12.7016 5.02132 13.6723 5.88772 14.5862C6.75412 15.5 8.14857 15.5 10.9375 15.5H12M16.2404 15.5C17.8014 15.5 18.5819 15.5 19.1336 15.0504C19.6853 14.6008 19.8429 13.8364 20.158 12.3075L20.6578 9.88275C21.0049 8.14369 21.1784 7.27417 20.7345 6.69708C20.2906 6.12 18.7738 6.12 17.0888 6.12H11.0235M4.95808 6.12H7"
            stroke="#1C274C"
            stroke-width="1.5"
            stroke-linecap="round"
          />
        </svg>
      </a>
      <a href="./" class="hover:underline">Login</a>
    </div>
    <button on:click={toggleMenu} class="md:hidden text-white">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        class="w-6 h-6"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M4 6h16M4 12h16m-7 6h7"
        />
      </svg>
    </button>
  </div>
  {#if $open}
    <div class="md:hidden mt-4 bg-pink-500">
      <a href="./" class="block p-2 hover:bg-pink-400">Wishlist</a>
      <a href="./" class="mr-4 hover:underline">
        <svg
          width="32px"
          height="32px"
          viewBox="0 0 32 32"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <g id="SVGRepo_bgCarrier" stroke-width="0" />

          <g
            id="SVGRepo_tracerCarrier"
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke="#000"
            stroke-width="6.4"
          >
            <g clip-path="url(#clip0_901_1512)">
              <path
                d="M8 16H2M9 20H3M7 12H1M26 16H11M25 20H12M27 12H10M10 24H27C28.125 24 28.862 23.038 29 22L31 8H6L4 1H1M13 29C13 27.896 12.104 27 11 27C9.896 27 9 27.896 9 29C9 30.104 9.896 31 11 31C12.104 31 13 30.104 13 29ZM25 29C25 27.896 24.104 27 23 27C21.896 27 21 27.896 21 29C21 30.104 21.896 31 23 31C24.104 31 25 30.104 25 29Z"
                stroke="#FFF"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </g>
            <defs>
              <clipPath id="clip0_901_1512">
                <rect width="32" height="32" fill="white" />
              </clipPath>
            </defs>
          </g>

          <g id="SVGRepo_iconCarrier">
            <g clip-path="url(#clip0_901_1512)">
              <path
                d="M8 16H2M9 20H3M7 12H1M26 16H11M25 20H12M27 12H10M10 24H27C28.125 24 28.862 23.038 29 22L31 8H6L4 1H1M13 29C13 27.896 12.104 27 11 27C9.896 27 9 27.896 9 29C9 30.104 9.896 31 11 31C12.104 31 13 30.104 13 29ZM25 29C25 27.896 24.104 27 23 27C21.896 27 21 27.896 21 29C21 30.104 21.896 31 23 31C24.104 31 25 30.104 25 29Z"
                stroke="#FFF6F9"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </g>
            <defs>
              <clipPath id="clip0_901_1512">
                <rect width="32" height="32" fill="white" />
              </clipPath>
            </defs>
          </g>
        </svg>
      </a>
      <a href="./" class="block p-2 hover:bg-pink-400">Login</a>
    </div>
  {/if}
</nav>
