<template>
  <div class="row">
    <div class="col-sm-12">
      <FormCheck @postTypeChange="postType = $event" />
      <table class="table" v-if="postType.length > 0">
        <thead>
          <tr>
            <th scope="col" v-show="postType.length === 2">#</th>
            <th scope="col">Title</th>
            <th scope="col">Body</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="post in postToShow"
            :key="post.id"
            :style="post.id % 2 === 0 ? 'background-color: white' : 'background-color: #f8f9fa'"
            @click="clickPost(post.id)"
            class="postRow"
          >
            <th scope="row" v-show="postType.length === 2">{{ post.id }}</th>
            <td>{{ post.title }}</td>
            <td>{{ post.body }}</td>
          </tr>
        </tbody>
      </table>
      <div v-else>
        <p>Please select one checkbox</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import FormCheck from "./FormCheck";
export default {
  name: "HelloWorld",
  components: {
    FormCheck
  },
  data() {
    return {
      posts: [],
      postType: []
    };
  },
  computed: {
    postToShow: function() {
      if (this.postType.length === 0) return [];

      let even = [];
      let odd = [];

      if (this.postType.findIndex(e => e === "even") !== -1) {
        even = this.posts.filter(function(post) {
          return post.id % 2 === 0;
        });
      }

      if (this.postType.findIndex(e => e === "odd") !== -1) {
        odd = this.posts.filter(function(post) {
          return post.id % 2 !== 0;
        });
      }

      return [...odd, ...even].sort(function(a, b) {
        return a.id - b.id;
      });
    }
  },
  methods: {
    clickPost: function(postId) {
      let post = this.posts.find(e => e.id === postId);
      post.title = post.title.substring(0, 20) + "...";
      post.body = post.body.substring(0, 50) + "...";
    }
  },
  created: function() {
    axios
      .get("https://jsonplaceholder.typicode.com/posts")
      .then(respose => {
        this.posts = respose.data;
      })
      .catch(error => console.log(error));
  }
};
</script>

<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.postRow {
  cursor: pointer;
}
</style>
