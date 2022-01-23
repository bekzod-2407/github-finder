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
      </div>
    </section>
  </div>
</template>

<script>
import search from "../components/Search.vue";
export default {
  components: { search },
  data() {
    return {
      search: "",
    };
  },
  computed: {
    message() {
      return this.$store.getters.getMsg;
    },
  },
  methods: {
      getRepos() {
          console.log(`got ${this.search} repository`);
      }
  }
};
</script>

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