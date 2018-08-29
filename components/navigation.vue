<template>
  <nav role="navigation" class="top-nav">
    <ul class="menu">
      <div class="columns is-multiline">
      <li v-if="!/\/.*?\/.*/.test(route.path)" v-for="route in this.$router.options.routes">
        <div class="button is-info is-8">
        <nuxt-link :to="{ name: route.name }">{{ route.name.charAt(0).toUpperCase() + route.name.slice(1) }}</nuxt-link>
        </div>
        
      </li>
</div>
    </ul>
  </nav>
</template>

<style>

  .person-name:link,
  .person-name:visited {
    width: 50%;
    display: inline-block;
    font-size: 2em;
    text-decoration: none;
    color: #373F49;
  }

  .top-nav {
    width: 50%;
    display: inline-block;
    text-align: right;
  }

  .menu {
    display: inline-block;
  }

  .menu li {
    display: inline-block;
    margin-left: 1em;
  }

  .menu a:link,
  .menu a:visited {
    color: #373F49;
    text-decoration: none;
  }

</style>
