<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <search
          :value="search"
          @search="search = $event"
          placeholder="Type username !"
        />

        <button @click="getRepos" class="btn btnPrimary">Search!</button>

        <!-- repository github -->

        <div class="repos__wrapper" v-if="repos">
          <div class="repos__item" v-for="repo in repos" :key="repo.id">
            <div class="repo__info">
              <a target="_blank" :href="repo.html_url">{{ repo.name }}</a>
              <span>{{ repo.stargazers_count }}⭐</span>
            </div>
          </div>
        </div>
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
      repos: null,
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
          console.log(res);
        })
        .catch((error) => {
          console.log(error);
        });
      console.log(`got ${this.search} repository`);
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
</style>