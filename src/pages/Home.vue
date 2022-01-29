<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <!-- error -->
        <div class="error">
          <p>{{ error }}</p>
        </div>
        <!-- search -->
        <search
          :value="search"
          @search="search = $event"
          placeholder="Type username !"
        />
        <button @click="getUser" class="btn btnPrimary">Get user</button>

        <button v-if="!repos" @click="getRepos" class="btn btnPrimary">
          Search github repository!
        </button>
        <button v-else @click="getRepos" class="btn btnPrimary">
          Search again!
        </button>

        <!-- repository github -->

        <div class="repos__wrapper" v-if="repos">
          <div class="repos__item" v-for="repo in repos" :key="repo.id">
            <div class="repos__info">
              <a class="repos__link" target="_blank" :href="repo.html_url">{{
                repo.name
              }}</a>
              <span>{{ repo.stargazers_count }}⭐</span>
            </div>
          </div>
        </div>

        <!-- user -->

        <!-- <div class="users__wrapper" v-if="users">
          <div class="users__item" v-for="user in users" :key="user.id">
            <div class="users__info">
              <div class="users-img">
                <img :src="user.avatar_url" :alt="user.login" />
              </div>
              <h1>{{ users.bio }}</h1>
            </div>
          </div>
        </div> -->
        <table v-if="users">
          <thead>
            <th>Name</th>
            <th>Avatar</th>
            <th>Login</th>
            <th>Repository</th>
            <th>followers</th>
            <th>following</th>
          </thead>
          <tbody>
            <tr v-for="user in users" :key="user.id">
              <td>
                {{ user.name }}
              </td>
              <td><img :src="user.avatar_url" :alt="user.name" /></td>
              <td>{{ user.login }}</td>
              <td>{{ user.public_repos }}</td>
              <td>{{ user.followers }}</td>
              <td>{{ user.following }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </div>
</template>

<script>
import search from "../components/Search.vue";
import axios from "axios";
export default {
  components: { search },
  data() {
    return {
      search: "",
      users: null,
      repos: null,
      error: null,
    };
  },
  computed: {
    message() {
      return this.$store.getters.getMsg;
    },
  },
  methods: {
    getRepos() {
      axios
        .get(`https://api.github.com/users/${this.search}/repos`)
        .then((res) => {
          this.repos = res.data;
          this.users = null;
          this.error = null;
          console.log(res);
        })
        .catch((error) => {
          this.repos = null;
          console.log(error);
          this.error = `User or repository was not found`;
        });
      console.log(`got ${this.search} repository`);
    },
    getUser() {
      axios
        .get(`https://api.github.com/users/${this.search}`)
        .then((resp) => {
          this.users = [];
          console.log("USERDATA", resp.data);
          this.users.push(resp.data);
          this.error = null;
          this.repos = null;
        })
        .catch((error) => {
          this.users = null;
          console.log(error);
          this.error = `User or repository was not found`;
        });
    },
  },
};
</script>
// https://api.github.com/users/bekzod-2407
<style lang="scss" scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.btnPrimary {
  margin-top: 40px;
}
.repos__wrapper,
.users__wrapper {
  //   border: 1px solid red;
  padding: 4px;
  width: 600px;
  margin: 30px 0;
}

.repos__info {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 10px 5px;
  border-bottom: 2px solid #dbdb;
}

.error {
  margin: 10px 0;
  text-align: center;
  font-size: 24px;
  color: rgb(160, 46, 46);
}

table {
  img {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    border: 2px solid #dbdb;
  }
}
</style>