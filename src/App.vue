<template>
  <h1>List of all JS repositories:</h1>
  <ListTable />
</template>

<script>
import ListTable from "./components/ListTable.vue";

export default {
  name: "App",
  components: {
    ListTable,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
