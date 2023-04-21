<script setup>
import Linguistical from './Linguistical.vue'
</script>

<template>
  <main>
    <h2>Demo</h2>
    <p>Theme: dark</p>
    <Linguistical user="AbleLincoln" repo="linguistical" theme="dark" />

    <p>Theme: light/none</p>
    <Linguistical user="AbleLincoln" repo="linguistical" />

    <h2>Usage</h2>
    <div class="codeblock">
      <code>
        {{`<Linguistical user="username" repo="repo" theme="dark | light" />`}}
      </code>
    </div>
  </main>
</template>

<style scoped>
main {
  max-width: 400px;
  margin: auto;

  font-family: sans-serif;
}

h2 {
  margin-top: 1.5em;
}

p {
  margin-top: 2em;
  margin-bottom: 0.5em;
}

.codeblock {
  background-color: rgb(45, 51, 59);
  color: white;
  padding: 10px;
  border-radius: 10px;
}
</style>
