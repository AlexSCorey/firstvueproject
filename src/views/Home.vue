<template>
  <div>
    <div class="row">
      <div class="col s6">
        <PostForm v-on:postCreated="addPost" :editingPost="editingPost" />
      </div>
      <div class="col s3">
        <p>Limit number of posts</p>
        <input type="number" v-model="postLimit" />
        <button @click="setLimit()" class="waves-effect waves-light btn">
          Set
        </button>
      </div>
    </div>
    <div class="row">
      <div
        class="col s6"
        v-for="(post, index) in posts"
        v-bind:item="post"
        v-bind:index="index"
        v-bind:key="post.id"
      >
        <div class="card">
          <div class="card-content">
            <p class="card-title">{{ post.title }}</p>
            <p class="timestamp">{{ post.createdAt | formatDate }}</p>
            <p>{{ post.body }}</p>
          </div>
          <div class="card-action">
            <a @click="editPost(post)" href="#">Edit</a>
            <a @click="deletePost(post.id)" href="#" class="delete-btn"
              >Delete</a
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import PostService from "../PostService";
import PostForm from "../components/PostForm.vue";
const postService = new PostService();

export default {
  name: "Home",
  components: {
    PostForm,
  },
  data() {
    return { posts: [], postLimit: 5, editingPost: null };
  },
  methods: {
    addPost(post) {
      const index = this.posts.findIndex((p) => p.id === post.id);
      if (index > -1) {
        this.posts.splice(index, 1, post);
      } else this.posts.unshift(post);
    },
    editPost(post) {
      this.editingPost = post;
    },
    deletePost(id) {
      postService
        .deletePost(id)
        .then(() => {
          this.posts = this.posts.filter((post) => post.id !== id);
        })
        .catch((error) => console.error(error));
    },
    setLimit() {
      postService
        .getPosts(this.postLimit)
        .then((res) => {
          this.posts = res.data;
        })
        .catch((error) => console.error(error));
    },
  },
  created() {
    postService
      .getAllPosts()
      .then((res) => {
        this.posts = res.data;
        console.log(this.posts);
      })
      .catch((error) => console.error(error));
  },
  filters: {
    formatDate(date) {
      date = new Date(date);
      const day = date.getDate();
      const month = date.getMonth() + 1;
      const year = date.getYear();
      return `${day}-${month}-${year}`;
    },
  },
};
</script>

<style scoped>
.card .card-content .card-title {
  margin-bottom: 0;
}
.card .card-content p.timestamp {
  margin-bottom: 10;
  color: #9999;
}
.delete-btn {
  color: red !important;
}
</style>
