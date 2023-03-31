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
          <NuxtLink to="flow">
            flow Page
          </NuxtLink>
        </li>
        <li>
          <NuxtLink to="tunnel">
            tunnel Page
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
            li {
                a {
                    color: rgb(131, 131, 131);
                    text-decoration: none;
                }
            }
        }
    }
}
</style>
