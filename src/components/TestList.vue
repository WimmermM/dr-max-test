<template>
  <div v-for="message in messages" :key="message.id">
    <h1 v-if="message.by">{{ message.by }}</h1>
    <h3 v-if="message.title">{{ message.title }}</h3>
    <p v-if="message.time">{{ new Date(message.time * 1000) }}</p>
    <p v-if="message.url">{{ message.url }}</p>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "TestList",
  data() {
    return {
      posts: [],
      messages: [{}],
      limit: 100,
    };
  },
  methods: {
    getPosts() {
      axios
        .get(
          "https://hacker-news.firebaseio.com/v0/topstories.json?print=pretty"
        )
        .then((resp) => {
          console.log(resp.data);
          this.posts = resp.data.slice(0, this.limit);
          console.log(this.posts);
          this.getItems();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getItems() {
      this.posts.forEach((post) => {
        axios
          .get(
            "https://hacker-news.firebaseio.com/v0/item/" +
              post +
              ".json?print=pretty"
          )
          .then((resp) => {
            this.messages.push(resp.data);
          })
          .catch((error) => {
            console.log(error);
          });
      });
      console.log(this.messages);
    },
  },
  created: function () {
    this.getPosts();
  },
};
</script>

<style scoped></style>
