<template>
  <div
    class="border border-8 border-gray p-6 m-4 md:w-[60%] lg:w-[40%] xl:w-[35%] md:m-auto md:my-8"
  >
    <div class="mb-4 flex flex-col sm:flex-row">
      <div class="w-[10rem] text-lg font-bold text-gray-600">EMAIL</div>
      <span class="text-gray-500 text-lg">{{ accountDetails.email }}</span>
    </div>
    <div class="mb-4 flex flex-col sm:flex-row">
      <div class="w-[10rem] text-lg w-30 font-bold text-gray-600">PASSWORD</div>
      <span class="text-gray-500 text-lg">{{ accountDetails.password }}</span>
    </div>
    <div class="mb-4 flex flex-col sm:flex-row">
      <div class="w-[10rem] text-lg w-30 font-bold text-gray-600">ROLE</div>
      <span class="text-gray-500 text-lg">{{ accountDetails.role }}</span>
    </div>
    <div class="mb-4 flex flex-col sm:flex-row">
      <div class="w-[10rem] text-lg w-30 font-bold text-gray-600">Skills</div>
      <span class="text-gray-500 text-lg">
        <span v-for="(skill, index) in accountDetails.skills" :key="index"
          >{{ skill
          }}<span v-if="index !== accountDetails.skills.length - 1"
            >,
          </span></span
        >
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["accountDetails"],
};
</script>