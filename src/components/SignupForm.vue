<template>
  <div class="border border-8 border-grey p-6 m-4 rounded md:w-[60%] lg:w-[40%] md:m-auto md:mt-8 ">
    <div class="mb-4">
      <label class="text-gray-400 font-bold block" for="email">EMAIL:</label>
      <input class="border border-grey my-2 px-3 py-2 w-[100%]" type="email" />
    </div>
    <div class="mb-4">
      <label class="text-gray-400 font-bold block" for="password"
        >PASSWORD:</label
      >
      <input class="border border-grey my-2 px-3 py-2 w-[100%]" type="email" />
    </div>
    <div class="mb-4">
      <label class="text-gray-400 font-bold block" for="Role">ROLE:</label>
      <select class="border border-grey my-2 px-3 py-2 w-[100%]">
        <option>Web Developer</option>
        <option>Web Designer</option>
      </select>
    </div>
    <div>
      <label class="text-gray-400 font-bold block" for="skills">SKILLS:</label>
      <input class="border border-grey my-2 px-3 py-2 w-[100%]" type="text" />
    </div>
    <div class="mb-4 flex flex-row gap-4 mt-2">
      <span class="bg-gray-200 px-4 py-1 rounded-2xl text-gray-500 font-bold"
        >html</span
      ><span class="bg-gray-200 px-4 py-1 rounded-2xl text-gray-500 font-bold"
        >css</span
      ><span class="bg-gray-200 px-4 py-1 rounded-2xl text-gray-500 font-bold"
        >vue</span
      >
    </div>
    <div class="mb-4 flex gap-2">
      <input id="checkbox" type="checkbox" />
      <label class="text-gray-400 font-bold text-xs" for="checkbox"
        >ACCEPT TERMS AND CONDITIONS</label
      >
    </div>
    <div class="flex justify-center">
      <button class="bg-blue-500 py-2 px-4 rounded-[3rem] text-white">
        Create an Account
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
};
</script>