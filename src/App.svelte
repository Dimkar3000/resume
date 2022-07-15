<script>
  import "./app.pcss";
  import { writable } from "svelte/store";
  import { fly } from "svelte/transition";
  import { onMount } from "svelte/internal";
  import NormaResume from "./NormalResume.svelte";
  import SpyrosResume from "./SpyrosResume.svelte";
  import NormalResume from "./NormalResume.svelte";

  let language = writable("en");

  onMount(() => {
    let urlParams = new URLSearchParams(location.search);
    language.set(urlParams.get("lang") ?? "en");
    document.body.dispatchEvent(new Event("view-ready"));
  });

  const selectLanguage = (lang) => {
    language.set(lang);
  };

  let language_menu_options = ["en", "gr"];
  $: lang_menu_open = false;

  let data;
  language.subscribe(async (lang) => {
    if (lang === "en") {
      let response = await fetch("data_en.json");
      data = await response.json();
    } else {
      let response = await fetch("data_gr.json");
      data = await response.json();
    }
  });
</script>

{#if data}
  <main class="mx-8 mt-2">
    <div
      class="flex flex-col gap-2 fixed right-2 bottom-2 rounded-lg print:hidden"
    >
      {#if lang_menu_open}
        {#each language_menu_options as option}
          <button
            transition:fly={{
              delay: 250,
              duration: 300,
              x: 100,
              // y: -500,
              opacity: 0.5,
            }}
            class={`h-12 w-12 border bg-gray-300 rounded-xl  text-2xl justify-center transition flex items-center align-middle text-center `}
            on:click={() => selectLanguage(option)}
          >
            {option}
          </button>
        {/each}
      {/if}
      <button
        class={`h-12 w-12 border bg-gray-300 rounded-xl font-extrabold text-4xl flex justify-center transition ${
          lang_menu_open ? "rotate-90" : ""
        }`}
        on:click={() => (lang_menu_open = !lang_menu_open)}
        on:focusout={(e) => (lang_menu_open = false)}
      >
        <svg
          class="w-6 h-6 self-center justify-self-center"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
          ><path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M15 19l-7-7 7-7"
          /></svg
        >
      </button>
    </div>
    <NormalResume {data} />
  </main>
{/if}
