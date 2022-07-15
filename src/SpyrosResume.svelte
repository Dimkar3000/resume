<script>
    import IoLogoLinkedin from "svelte-icons/io/IoLogoLinkedin.svelte";
    import IoMdPhonePortrait from "svelte-icons/io/IoMdPhonePortrait.svelte";
    import IoIosMail from "svelte-icons/io/IoIosMail.svelte";
    export let data;
</script>

<section class="mx-auto w-full text-center mt-36 font-mono">
    <h1 class="font-extrabold text-3xl text-blue-400">{data.name}</h1>
    <h3 class="text-gray-500 text-sm font-bold">{data.location}</h3>
    <ul
        class="text-gray-500 text-sm font-bold flex items-center  justify-center align-middle space-x-4  mx-auto"
    >
        <li class="flex whitespace-nowrap items-center">
            <div class="h-6"><IoMdPhonePortrait /></div>
            {data.phone}
        </li>
        <li class="flex whitespace-nowrap items-center">
            <div class="h-6 "><IoIosMail /></div>
            {data.email}
        </li>

        <li class="flex whitespace-nowrap items-center">
            <div class="h-6"><IoLogoLinkedin /></div>
            {data.location}
        </li>
    </ul>
</section>
<p class="mt-10 text-sm font-normal">
    {data.profile_content}
</p>

<section>
    <h2
        class="w-full border-b-2 text-blue-500 border-blue-500 border-opacity-50 font-bold text-2xl"
    >
        Academic qualifications
    </h2>
    <h2>{data.university_title}</h2>
</section>
