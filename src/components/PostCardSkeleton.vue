<template>
  <div class="rounded border md:shadow w-full md:w-64 overflow-hidden">
    <!-- image post -->
    <div class="flex justify-center">
      <AnimatedSkeleton class="w-full h-56" />
    </div>

    <div class="px-4 py-4">
      <!-- owner post -->
      <div class="flex flex-row mb-4">
        <AnimatedSkeleton class="w-8 h-8 rounded-full inline-block mr-2" />
        <AnimatedSkeleton class="inline-block w-full mb-2" />
      </div>

      <!-- text post -->
      <AnimatedSkeleton class="w-full mb-2" />

      <!-- tag post -->
      <AnimatedSkeleton class="w-full mb-2" />
    </div>
  </div>
</template>

<script setup>
import AnimatedSkeleton from './AnimatedSkeleton.vue';


</script>
