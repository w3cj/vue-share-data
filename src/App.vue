<template>
  <div id="app" class="container">
    <header class="page-header">
      <h4>Ron Swanson Quotes</h4>
      <router-link class="btn btn-success" :to="'/'">Quotes</router-link>
      <router-link class="btn btn-primary" :to="'/view'">View</router-link>
    </header>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'app',
};
</script>

<style>
</style>
