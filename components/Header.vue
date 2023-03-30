<template>
  <header class="header">
    <nav>
      <ul>
        <li>
          <NuxtLink to="/">
            Home Page
          </NuxtLink>
        </li>
        <li>
          <NuxtLink to="test1">
            test1 Page
          </NuxtLink>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script setup lang="ts">

</script>

<style lang="scss">
.header {
    position: fixed;
    right: 0;
    top: 0;
    nav {
        ul {
            display: flex;
            flex-wrap: wrap;
            list-style: none;
            gap: 20px;
        }
    }
}
</style>
