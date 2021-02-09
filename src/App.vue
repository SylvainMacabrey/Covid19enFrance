<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg navbar-dark primary-color">
      <a class="navbar-brand" href="">Covid-19 en France</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#basicExampleNav"
        aria-controls="basicExampleNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
    </nav>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>

</style>
