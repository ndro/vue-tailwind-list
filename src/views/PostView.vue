<template>
  <div class="container">
    <!-- form input single tag -->
    <div class="flex flex-col sm:flex-row">
      <div class="relative py-4">
        <div class="absolute left-0 inset-y-0 pl-3 flex items-center">
          <svg class="fill-current h-6 w-6 text-gray-400" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M12.9 14.32a8 8 0 1 1 1.41-1.41l5.35 5.33-1.42 1.42-5.33-5.34zM8 14A6 6 0 1 0 8 2a6 6 0 0 0 0 12z" /></svg>
        </div>
        <input
          class="border pl-12 pr-4 py-2 rounded-full focus:border-blue-500 focus:shadow-outline outline-none" 
          type="text" 
          placeholder="Search by tag (single tag)"
          v-model="singleTag"
          @input="getPostList(), resetPage()"
        />
      </div>
      <!-- spinner loading -->
      <div class="sm:py-6 flex justify-center sm:pl-4" v-if="isLoad">
        <div
          class="inline-block h-6 w-6 animate-spin rounded-full border-4 border-solid border-current border-r-transparent align-[-0.125em] motion-reduce:animate-[spin_1.5s_linear_infinite]"
          role="status">
        </div>
      </div>
    </div>

    <div class="flex flex-row flex-wrap gap-6 justify-evenly" v-if="isLoad">
      <PostCardSkeleton /> <PostCardSkeleton />
      <PostCardSkeleton /> <PostCardSkeleton />
      <PostCardSkeleton /> <PostCardSkeleton />
      <PostCardSkeleton /> <PostCardSkeleton />
    </div>
    <!-- content post -->
    <div class="flex flex-row flex-wrap gap-6 justify-evenly" v-else-if="postList">
      <div v-for="post in postList.data" :key="post.id" class="flex-1 md:flex-none">
        <PostCard :post="post" @click-tag="clickTag" />
      </div>
    </div>

    <!-- pagination -->
    <div v-if="postList">
      <paginate
        v-model="page"
        :page-count="pageCount"
        :page-range="3"
        :margin-pages="1"
        :click-handler="clickCallback"
        :prev-text="'Prev'"
        :next-text="'Next'"
        :container-class="'md:mb-8 pt-8 px-2 sm:px-4 mx-auto flex justify-center select-none'"
        :page-link-class="'block border px-2 sm:px-4 py-1 hover:bg-gray-200 text-gray-600 cursor-pointer'"
        :prev-link-class="'block border px-2 sm:px-4 py-1 rounded-l hover:bg-gray-200 text-gray-600 cursor-pointer'"
        :next-link-class="'block border px-2 sm:px-4 py-1 rounded-r hover:bg-gray-200 text-gray-600 cursor-pointer'"
        :active-class="'hover:bg-blue-500 bg-blue-500 text-white cursor-default'"
        :no-li-surround="true"
      >
      </paginate>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios'
import PostCard from '../components/PostCard.vue';
import Paginate from "vuejs-paginate-next";
import PostCardSkeleton from '../components/PostCardSkeleton.vue';

// env variable
const url = import.meta.env.VITE_URL
const app_id = import.meta.env.VITE_APP_ID

const singleTag = ref('');
const queryTimeout = ref(null);
const postList = ref(null);
const isLoad = ref(false);
const isReset = ref(false);

const getPostList = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    isLoad.value = true
    if (isReset.value) page.value = 1 // set page to 1 if singleTag is change

    // post with single tag
    if (singleTag.value !== '') {
      try {
        const postData = await axios.get(`${url}/tag/${singleTag.value}/post?limit=${limit.value}&page=${page.value - 1}`,{
          headers: {
            'app-id': app_id
          }
        })

        total.value = postData.data.total
        pageCount.value = Math.round(total.value / limit.value)

        postList.value = postData.data
      } catch (error) {
        console.log(error);
      }
      isLoad.value = false
      isReset.value = false
      return;
    }

    // default post without single tag
    try {
      const postData = await axios.get(`${url}/post?limit=${limit.value}&page=${page.value - 1}`,{
        headers: {
          'app-id': app_id
        }
      })

      total.value = postData.data.total
      pageCount.value = Math.round(total.value / limit.value)

      postList.value = postData.data
    } catch (error) {
      console.log(error);
    }
    isLoad.value = false
    isReset.value = false
    return;
  }, 500);
}

// get post by click tag
const clickTag = (tag) => {
  singleTag.value = tag;
  resetPage()
  getPostList()
}

// pagination
const limit = ref(8);
const page = ref(1);
const total = ref(0);
const pageCount = ref(0);

const clickCallback = (pageNum) => {
  page.value = pageNum;
  getPostList()
}

// reset page if singleTag is change
const resetPage = () => {
  isReset.value = true
}

// first load
onMounted(() => {
  getPostList()
})
</script>
