<template>
  <div class="absolute -bottom-64 lg:-bottom-20 flex flex-col
  lg:flex-row bg-white gap-y-4 gap-x-12 px-8 pt-8 pb-12 rounded-md">
    <!-- Flex Items #1 -->
    <div class="flex flex-col">
      <h3 class="text-xs mb-2 uppercase">IP Address</h3>
      <span class="text-2xl">{{ipData?.ip}}</span>
    </div>
    <!-- Flex Items #2 -->
    <div class="flex flex-col">
      <h3 class="text-xs mb-2 uppercase">City</h3>
      <span class="text-2xl capitalize">{{ipData?.address?.city || ipData?.city}}</span>
    </div>
    <!-- Flex Items #3 -->
    <div class="flex flex-col">
      <h3 class="text-xs mb-2 uppercase">State</h3>
      <span class="text-2xl">{{ipData?.address?.region || ipData?.state}}</span>
    </div>
    <!-- Flex Items #4 -->
    <div class="flex flex-col">
      <h3 class="text-xs mb-2 uppercase">Country</h3>
      <span class="text-2xl">{{ipData?.address?.country || ipData?.country}}</span>
    </div>
  </div>
</template>

<script>
export default {
props: ['ipData']
}
</script>