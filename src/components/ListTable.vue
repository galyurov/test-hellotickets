<template>
  <AnimatedLoader v-if="loading" />
  <div v-else class="list-table">
    <div class="table">
      <div class="table-head">
        <span class="table-head-title"> Name </span>
        <span class="table-head-title"> Stars </span>
        <span class="table-head-title"> Url </span>
      </div>
      <div class="table-body">
        <div
          v-for="repo in repositories"
          :key="repo.id"
          class="table-body-item"
        >
          <span class="item-text">{{ repo.name }}</span>
          <span class="item-text">{{ repo.stargazers_count }}</span>
          <a :href="repo.html_url" target="_blank" class="item-text">{{
            repo.url
          }}</a>
        </div>
      </div>
      <div class="table-pagination">
        <div v-if="pageNumber > 1" @click="pageNumber--" class="button">
          {{ pageNumber - 1 }}
        </div>
        <div class="button current-page">{{ pageNumber }}</div>
        <div @click="pageNumber++" class="button">
          {{ pageNumber + 1 }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Octokit } from "octokit";
import AnimatedLoader from "./AnimatedLoader.vue";

const oktokit = new Octokit({
  auth: process.env.VUE_APP_SECRET_CODE,
});

export default {
  components: { AnimatedLoader },
  name: "ListTable",
  data() {
    return {
      pageNumber: 1,
      repositories: [],
      loading: true,
    };
  },
  mounted() {
    this.getRepos(this.pageNumber);
  },

  methods: {
    async getRepos(page) {
      this.loading = true;
      const { data } = await oktokit.request(
        "GET /search/repositories{?q,sort,per_page,page}",
        {
          q: encodeURI("language:JavaScript"),
          sort: "stars",
          per_page: 30,
          page,
        }
      );
      this.loading = false;
      this.repositories = data.items;
    },
  },
  watch: {
    pageNumber() {
      this.getRepos(this.pageNumber);
    },
  },
};
</script>

<style lang="scss" scoped>
.table {
  &-head {
    font-size: 18px;
    display: grid;
    grid-template-columns: 3fr 1fr 3fr;
    border: 2px solid grey;
    border-bottom: none;
    border-radius: 6px 6px 0 0;
  }
  &-head-title {
    padding: 6px 8px;
    font-weight: 700;
  }
  &-body {
    border: 2px solid grey;
  }
  &-body-item {
    display: grid;
    grid-template-columns: 3fr 1fr 3fr;
    & .item-text {
      padding: 2px;
      color: black;
      border-bottom: 1px solid black;
    }
  }
  &-pagination {
    display: flex;
    justify-content: center;
    font-size: 18px;
    margin-top: 10px;
    .button {
      padding: 5px 10px;
      margin: 0 5px;
      border-radius: 2px;
      cursor: pointer;
      &:hover {
        background: lightgreen;
      }
    }

    .current-page {
      font-weight: 600;
      background: lightgrey;
    }
  }
}
</style>
