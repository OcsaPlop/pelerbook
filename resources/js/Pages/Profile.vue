<template>
  <Head
    :title="profile.firstname + ' ' + profile.surname + ` | Pelerbook`"
  ></Head>
  <div
    class="bg-[#242526] h-32 p-4 rounded-lg mt-6 max-w-lg mx-auto"
    v-if="auth.user.id == profile.id"
  >
    <div class="flex gap-4 border-b pb-4 border-[#3a3b3c]">
      <Link class="aspect-square h-12" :href="'/' + auth.user.id">
        <img
          :src="auth.user.avatar"
          class="rounded-full object-cover w-12 h-12"
        />
      </Link>
      <button
        @click="showCreatePost"
        class="bg-[#3a3b3c] hover:bg-[#4e4f50] w-full rounded-full px-6 py-3 text-start"
        v-html="`What's on your mind, ` + auth.user.firstname + `?`"
      />
    </div>
  </div>
  <div
    class="fixed top-0 w-screen h-screen flex justify-center items-center bg-black/50 z-50"
    v-if="createPost"
    id="post"
  >
    <CreatePost :firstname="auth.user.firstname" @close="showCreatePost" />
  </div>

  <div
    v-for="post in posts.data"
    class="card max-w-lg bg-[#242526] shadow-xl mx-auto mt-4"
    :key="post.id"
  >
    <Post :post="post" :auth="auth" />
  </div>
</template>
<script setup lang="ts">
import { ref } from "vue";
import CreatePost from "../Shared/CreatePost.vue";
import Post from "../Shared/Post.vue";
import type { UserType } from "@/UserType.ts";
import type { AuthType } from "@/AuthType";
import type { PostsType } from "@/PostsType";

const { posts, auth, profile } = defineProps<{
  posts: PostsType;
  auth: AuthType;
  profile: UserType;
}>();

const createPost = ref<boolean>(false);

const showCreatePost = () => {
  createPost.value = !createPost.value;
  if (createPost.value) {
    return document.body.classList.add("overflow-hidden");
  }
  document.body.classList.remove("overflow-hidden");
};
</script>
