<template>
  <div class="container">
    <h1>Latest Posts</h1>
    <label for="" class="post-label">Post something!</label>
    <br>
    <input type="text" id="update-post" v-model="postText" @keyup.enter="updatePost()">
    <button @click="updatePost()">
      Post
    </button>
    <hr />
    <p class="error" v-if="error">{{ error }}</p>

    <div class="post-container" v-if="posts">
      <div
        class="post"
        v-for="(post, index) in posts"
        :key="index"
        :item="post"
      >
      <a class="delete-post" href="" @click.prevent="deletePost(post._id)">Delete</a>
        <p>{{
          `${post.createdAt.getDate()}/${
            post.createdAt.getMonth() + 1
          }/${post.createdAt.getFullYear()}`
        }}</p>
        <p class="text">{{ post.name }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import PostService from "../postService.js";

export default {
  name: "PostComponent",
  data() {
    return {
      posts: [],
      error: "",
      postText: "",
    };
  },
  methods: {
    async updatePost() {
      try {
        await PostService.insertPost(this.postText);
        this.posts = await PostService.getPosts();
      } catch (err) {
        this.error = err.message;
      }
    },
    async deletePost(id) {
      try {
        await PostService.deletePost(id);
        this.posts = await PostService.getPosts();
      } catch (err) {
        this.error = err.message;
      }
    }
  },
  async created() {
    try {
      this.posts = await PostService.getPosts();
    } catch(err) {
      this.error = err.message;
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.error {
  color:red;
}
.post-container {
  display: inline-block;
  width: 100%;
}
.post {
  color: #2b8430;
  padding: 2px 0;
  margin-top: 1%;
  background-color: #a3fda8;
  border:2px solid;
}
.post p {
    color: #1d3a1f;
    font-weight: 600;
  }
.delete-post {
  text-align: right;
  display: grid;
}
</style>
