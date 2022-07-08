<script>
  import "./app.pcss";
  import TiMail from "svelte-icons/ti/TiMail.svelte";
  import MdLocationOn from "svelte-icons/md/MdLocationOn.svelte";
  import MdPhone from "svelte-icons/md/MdPhone.svelte";
  import IoLogoLinkedin from "svelte-icons/io/IoLogoLinkedin.svelte";
  import DiStackoverflow from "svelte-icons/di/DiStackoverflow.svelte";
  import DiGithubBadge from "svelte-icons/di/DiGithubBadge.svelte";
  import FaUser from "svelte-icons/fa/FaUser.svelte";
  import FaSuitcase from "svelte-icons/fa/FaSuitcase.svelte";
  import { writable } from "svelte/store";
  import { fly } from "svelte/transition";
  import { each, onMount } from "svelte/internal";

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
    <section class=" w-full flex">
      <div class="mr-4 my-auto h-32 w-32 rounded-xl flex-shrink-0">
        <img
          alt=""
          src={data.logo}
          class="h-full object-cover mx-auto overflow-hidden rounded-2xl"
        />
      </div>
      <div class="my-10">
        <p class="font-bold text-2xl capitalize">{data.name}</p>
        <span class="font-light capitalize text-gray-500">
          {data.university_title}
        </span>
      </div>
    </section>

    <section
      class="-mx-8 mb-4 px-8 bg-gray-100 grid grid-cols-2 h-32 place-content-evenly item"
    >
      <div class="h-10 flex items-start">
        <div class="w-8">
          <TiMail />
        </div>
        <span class=" ml-2 place-self-center">{data.email}</span>
      </div>

      <div class="h-10 flex">
        <div class="w-8">
          <MdPhone />
        </div>
        <span class=" ml-2 flex-1 place-self-center">{data.phone}</span>
      </div>

      <div class="h-10 flex">
        <div class="w-8">
          <MdLocationOn />
        </div>
        <span class=" ml-2 flex-1 place-self-center">{data.location}</span>
      </div>

      <div class="h-10 flex">
        <div class="w-8">
          <IoLogoLinkedin />
        </div>
        <span class=" ml-2 flex-1 place-self-center"
          >{data.linkedIn_username}</span
        >
      </div>
      <div class="h-10 flex">
        <div class="w-8">
          <DiGithubBadge />
        </div>
        <span class=" ml-2 flex-1 place-self-center"
          >{data.github_username}</span
        >
      </div>

      <div class="h-10 flex">
        <div class="w-8">
          <DiStackoverflow />
        </div>
        <span class=" ml-2 flex-1 place-self-center"
          >{data.stackoverflow_username}</span
        >
      </div>
    </section>

    <section class="grid grid-cols-3 gap-2">
      <section class="col-span-2 mr-8">
        <div class="text-2xl mb-4 font-bold flex items-end">
          <div class="w-8 h-8 mr-2">
            <FaUser />
          </div>
          <p class="capitalize">{data.profile_title}</p>
        </div>

        <article class="mb-2 text-xs">
          {data.profile_content}
        </article>

        <div class="text-2xl mb-4 font-bold flex items-end">
          <div class="w-8 h-8 mr-2">
            <FaSuitcase />
          </div>
          <p>{data.emplyment_title}</p>
        </div>
        {#each data.emplyment_content as story}
          <div class="font-semibold text-2xl capitalize">
            {story.workTitle}
          </div>
          <div class="font-medium text-xl">{story.company}</div>
          <div class="flex justify-between">
            <div class="font-light text-sm">{story.from} - {story.to}</div>
            <div class="font-light text-sm">{story.from} - {story.to}</div>
          </div>
          <ul class="text-justify text-xs">
            {#each story.items as item}
              <li>- {@html item}</li>
            {/each}
          </ul>
        {/each}
      </section>
      <section class="flex flex-col items-end">
        {#each data.bullet_points as item}
          <div class="flex flex-col mb-4 w-64">
            <ul class="text-sm font-light ">
              <p class="font-semibold mb-2 ">{item.title}</p>
              {#each item.listItems as listItem}
                <li>{listItem}</li>
              {/each}
            </ul>
          </div>
        {/each}
      </section>
    </section>
  </main>
{/if}
