<template>
  <div class="flex flex-col min-h-screen">
    <SiteNavigation />
    <main>
      <Suspense>
        <PostView />
      </Suspense>
    </main>
  </div>
</template>

<script setup>
import PostView from './views/PostView.vue';
import SiteNavigation from './components/SiteNavigation.vue'
</script>
