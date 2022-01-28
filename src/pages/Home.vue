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

        <Vuetable
          v-if="users"
          :api-mode="false"
          :data="users"
          :fields="UserFields"
           pagination-path=""
        />
        <template slot="images" slot-scope="props">
          <div class="img__wrapper">
            <img
              :src=" props.rowData.image[0].name"
              alt=""
            />
          </div>
        </template>

        <!-- <div class="users__wrapper" v-if="users">
          <div class="users__item" v-for="user in users" :key="user.id">
            <div class="users__info">
              <div class="users-img">
                <img :src="user.avatar_url" :alt="user.login" />
              </div>
              <h1>{{ users.bio }}</h1>
              <p>{ {}}</p> -->
        <!-- <span>{{ repo.stargazers_count }}⭐</span> -->
        <!-- </div>
          </div>
        </div> -->
      </div>
    </section>
  </div>
</template>

<script>
import search from "../components/Search.vue";
import axios from "axios";
import Vuetable from "vuetable-2";
import { UserFields } from "../vuetable/vuetable-fields";
export default {
  components: { search, Vuetable },
  data() {
    return {
      UserFields,
      search: "",
      users: [],
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

.repos__info,
.users__info {
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

.users-img {
  img {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    border: 2px solid #dbdb;
  }
}
</style>