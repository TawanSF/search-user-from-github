<template>
    <div class="">
        <h1>{{ msg }}</h1>
        <div>
          <input type="text" placeholder="Username" v-model="username">
        </div>
        <br>
        <div>
            <button class="btn btn-sm btn-success" @click.prevent.stop="getIssues()">Search</button>
            <button class="btn btn-sm btn-danger" @click.prevent.stop="reset()">Limpar</button>
        </div>
        <hr>
        <div class="table-section">
          <table style="width:100%" class="table">
            <tr>
                <th>Id</th>
                <th>Username</th>
                <th>Blog</th>
                <th>Profile</th>
                <th>Location</th>
                <th>Bio</th>
            </tr>
            <tr v-if="loader.getUser" colspan="5" class="loader">
              <td><img src="https://loading.io/spinners/infinity/lg.infinity-rotate-cycle-loader.gif" alt="Loading..."></td>
            </tr>
            <tr v-if="users != null && !loader.getUser">
              <td>{{ users.id }}</td>
              <td>{{ users.login }}</td>
              <td><a v-bind:href="users.blog">{{ users.blog }}</a></td>
              <td><a v-bind:href="users.html_url">{{ users.html_url }}</a></td>
              <td>{{ users.location }}</td>
              <td>{{ users.bio }}</td>
            </tr>
            <tr v-if="users == null && !loader.getUser">
              <td>User not found</td>
            </tr>
          </table>
        </div>
    </div>
</template>

<script>
/* eslint-disable */
import axios from 'axios';
export default {
  name: 'HelloWorld',
  data() {
    return {
      msg: 'Search User',
      username: '',
      users: [],
      selectHtmlUrl : {},
      selectBlog: {},
      loader: {
        getUser: false
      }
    };
  },
  methods: {
    reset() {
        this.username = '';
    },
    getIssues() {
      if (this.username) {
        this.loader.getUser = true;
        const url = `https://api.github.com/users/${this.username}`;
        axios.get(url).then((response) => {
          this.users = response.data;
        }).finally(() => {
          this.loader.getUser = false;
        })
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
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
.loader {
  text-align: center;
}
</style>
