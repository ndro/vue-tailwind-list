<template>
  <div class="rounded border md:shadow w-full md:w-64 overflow-hidden">
    <!-- image post -->
    <div class="flex justify-center">
      <a href="#">
        <img
          class="object-cover object-center h-56"
          :src="post.image"
          :alt="post.image"
        />
      </a>
    </div>

    <div class="px-4 py-4">
      <!-- owner post -->
      <div class="text-xs text-gray-800 font-medium">
        <a href="#">
          <img
            class="w-8 h-8 rounded-full inline-block mr-2"
            :src="post.owner.picture"
            :alt="post.owner.firstName"
          />
          <span class="uppercase truncate font-bold">
            {{ post.owner.firstName }} {{ post.owner.lastName }}
          </span>
        </a>
      </div>

      <!-- text post -->
      <p class="text-gray-800 my-2 md:h-12 hover:underline text-md line-clamp-2">
        <a href="#">{{ post.text }}</a> 
      </p>

      <!-- tag post -->
      <div class="text-sm text-gray-800 truncate">
        <span v-for="(tag, index) in post.tags" :key="index">
          #{{ tag }}
        </span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  post: {
    type: Object,
    default: () => {}
  }
})
</script>
